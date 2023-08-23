Brave uses Rust language code, but there are some caveats.

## Review guidelines

A checklist for reviewing Rust code changes, including dependent library crates.

* Third-party dependency versions should be aligned with what's already in the upstream chromium or brave trees. If a change needs to update one of those crates, make sure there's at least an issue open for all other users to coordinate migration so we don't ship duplicate code.
* Carefully check any `thread_local` use. Leaf code may be called from different C++ threads in sequence, so thread-local variables won't be consistent.
* `build.rs` is forbidden. We don't use `cargo` to build Rust code, so many affordances there don't work. Any custom artefact generation would need to be ported to `BUILD.gn`.
* Any use of `autocfg` in dependencies must be patched out and replaced by config settings in `BUILD.gn` based on variables there. Since we don't use cargo, these checks won't work.