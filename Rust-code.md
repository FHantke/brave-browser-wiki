Brave uses Rust language code, but there are some caveats.

Since 1.58 brave-core has used the upstream chromium build system for rust code. All code must be in-tree, with 3rd-party crates (libraries) vendored under either `brave/third_party/rust` or upstream in `src/third_party/rust`.
We largely follow the [chromium guidelines](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/adding_to_third_party.md#Rust) for determining acceptable use, although we have some components (SKUs, Speedreader, STAR) implemented in rust. Please contact Clifton and Brian Johnson when planning any new work involving substantial Rust code.

There is a `gnrt` tool included with chromium which helps maintain the rust sub-tree. It extracts information from Cargo.toml files, downloads any referenced third-party packages and generates an equivalent BUILD.gn, which must then be checked into the tree. See the [instructions](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/rust.md#Updating-existing-third_party-crates) for how to build and run this tool.

Instead of `cargo`, our build calls `rustc` directly based on the description in the accompanying BUILD.gn files. However, cargo tooling can often be used for local testing and verification.

## Updating a dependency

Since brave v1.65.79 we have patched `gnrt` to support our two-level third_party/rust tree. To update depencencies, change the required version in the relevant Cargo.toml and/or update the project-level `third_party/rust/chromium_crates_io/Cargo.lock` e.g. by running `cargo update -p <depname>` in that directory.
Then run `gnrt vendor` to update the third-party source packages followed by `gnrt gen` to update the build description and metadata. Review and commit the changes. Note that `gnrt vendor` will remove obsolete packages as well as adding new ones.

gnrt currently expects to be invoked from the top-level (chromium) `src` directory and won't work from inside the brave-core repository.

To reduce code duplication, please try to always move toward unifying different versions of dependencies, and make use of any packages already included in upstream chromium whenever possible.

### Details for manual review

If you're interested in the details, or need to hack something until the automation can be improved to handle it, the general scheme is:

- third_party packages are downloaded and unpacked into `third_party/rust/chromium_crates_io/vendor/$CRATE-$VERSION`
- packages previously downloaded by the system cargo can be found in `$HOME/.cargo/registry/src` if you want to compare or copy from there
- package download urls can be fetched manually from e.g. `https://crates.io/api/v1/crates/$package/$version/download`
- While the vendored source and top-level Cargo.toml is under chromium_crates_io/vendor (for compatibility with cargo-vendor) each package must have a separate BUILD.gn and README.chromium in `third_party/rust/$package/$epoch/` where `$epoch` is a flattened version number. E.g. `serde-1.0.193 -> serde/v1`, `ppoprf-0.3.1` -> `ppoprf/v0_3`.

### Checking your work

Since the process involves a lot of manual tedium and is error-prone either way, it's a good idea to compare your work to a known-good PR.

#### Updating a patch version
[This PR](https://github.com/brave/brave-core/pull/20113/files) updates `adblock 0.8.0` to `adblock 0.8.1` without any `brave-core` code changes.

#### Updating a minor version
[This PR](https://github.com/brave/brave-core/pull/19648/files) updates `adblock 0.7` to `adblock 0.8`. This is generally similar to the patch version diff, but there are additional related changes (i.e. see `components/brave_shields/adblock/rs/BUILD.gn` where a path is changed), along with additional unrelated changes (`brave-core` code changes to account for the newer API).

## Review guidelines

A checklist for reviewing Rust code changes, including dependent library crates.

* Third-party dependency versions should be aligned with what's already in the upstream chromium or brave trees. If a change needs to update one of those crates, make sure there's at least an issue open for all other users to coordinate migration so we don't ship duplicate code.
* Carefully check any `thread_local` use. Leaf code may be called from different C++ threads in sequence, so thread-local variables won't be consistent.
* `build.rs` is forbidden. We don't use `cargo` to build Rust code, so many affordances there don't work. Any custom artefact generation would need to be ported to `BUILD.gn`.
* Any use of `autocfg` in dependencies must be patched out and replaced by config settings in `BUILD.gn` based on variables there. Since we don't use cargo, most of these checks won't work, or will produce the wrong values. See [here](https://github.com/brave/brave-core/commit/29fc07ef291593b5c4b9b2587ca184ab3d890650) for an example handling these issues.