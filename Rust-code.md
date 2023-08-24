Brave uses Rust language code, but there are some caveats.

Since 1.58 brave-core has used the upstream chromium build system for rust code. All code must be in-tree, with 3rd-party crates (libraries) vendored either in `brave/third_party/rust` or upstream in `src/third_party/rust`.
We largely follow the [chromium guidelines](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/adding_to_third_party.md#Rust) for determining acceptable use, although we have some components (SKUs, Speedreader, STAR) implemented in rust. Please contact Clifton and Brian Johnson when planning any new work involving substantial Rust code.

Instead of `cargo`, our build calls `rustc` directly. Source files, config settings, and dependencies are extracted from Cargo.toml files and an equivalent BUILD.gn is created and checked into the tree. There is a `gnrt` tool to automate this, but it is currently very rough and doesn't support our setup of with `third_party/rust` trees. See the [instructions](https://chromium.googlesource.com/chromium/src/tools/+/refs/heads/main/crates/README.md) for how to build and run this tool.

## Review guidelines

A checklist for reviewing Rust code changes, including dependent library crates.

* Third-party dependency versions should be aligned with what's already in the upstream chromium or brave trees. If a change needs to update one of those crates, make sure there's at least an issue open for all other users to coordinate migration so we don't ship duplicate code.
* Carefully check any `thread_local` use. Leaf code may be called from different C++ threads in sequence, so thread-local variables won't be consistent.
* `build.rs` is forbidden. We don't use `cargo` to build Rust code, so many affordances there don't work. Any custom artefact generation would need to be ported to `BUILD.gn`.
* Any use of `autocfg` in dependencies must be patched out and replaced by config settings in `BUILD.gn` based on variables there. Since we don't use cargo, these checks won't work.