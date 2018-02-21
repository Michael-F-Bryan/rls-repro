# RLS Error Minimal Example

A minimal example to help reproduce [rust-lang-nursery/rls#722].

Compiler versions and compilation target:

```text
$ cargo --version
cargo 0.26.0-nightly (1d6dfea44 2018-01-26)
$ rustc --version
rustc 1.25.0-nightly (27a046e93 2018-02-18)
$ rustup show | grep '(default)'
nightly-x86_64-pc-windows-msvc (default)
```

Generate a trivial project with two sub-crates and a virtual `Cargo.toml` at the
top.

```text
$ mkdir rls-repro
$ cd rls-repro
$ cargo new crate_a
$ cargo new crate_b
$ vim Cargo.toml
...
$ cat Cargo.toml
[workspace]
members = ["crate_a", "crate_b"]
```

Compiling and `cargo check` from the command-line works just fine.

```text
$ cargo check
cargo check --verbose
   Compiling crate_b v0.1.0 (file:///C:/Users/mbryan/AppData/Local/Temp/rls-repro/crate_b)
   Compiling crate_a v0.1.0 (file:///C:/Users/mbryan/AppData/Local/Temp/rls-repro/crate_a)
     Running `rustc --crate-name crate_b crate_b\src\lib.rs --crate-type lib --emit=dep-info,metadata -C debuginfo=2 -C metadata=8d51d4c3e0b38fc4 -C extra-filename=-8d51d4c3e0b38fc4 --out-dir C:\Users\mbryan\AppData\Local\Temp\rls-repro\target\debug\deps -C incremental=C:\Users\mbryan\AppData\Local\Temp\rls-repro\target\debug\incremental -L dependency=C:\Users\mbryan\AppData\Local\Temp\rls-repro\target\debug\deps`
     Running `rustc --crate-name crate_a crate_a\src\lib.rs --crate-type lib --emit=dep-info,metadata -C debuginfo=2 -C metadata=56c0262484737d1f -C extra-filename=-56c0262484737d1f --out-dir C:\Users\mbryan\AppData\Local\Temp\rls-repro\target\debug\deps -C incremental=C:\Users\mbryan\AppData\Local\Temp\rls-repro\target\debug\incremental -L dependency=C:\Users\mbryan\AppData\Local\Temp\rls-repro\target\debug\deps`
    Finished dev [unoptimized + debuginfo] target(s) in 0.34 secs
$ cargo build
   Compiling crate_a v0.1.0 (file:///C:/Users/mbryan/AppData/Local/Temp/rls-repro/crate_a)
   Compiling crate_b v0.1.0 (file:///C:/Users/mbryan/AppData/Local/Temp/rls-repro/crate_b)
    Finished dev [unoptimized + debuginfo] target(s) in 0.35 secs
```

These are all the rust-specific options in my VS Code user settings:

```json
{
    "rust-client.updateOnStartup": true,
    "rust.goto_def_racer_fallback": true,
    "rust.wait_to_build": 1000,
    "rust.build_on_save": true
}
```

[rust-lang-nursery/rls#722]: https://github.com/rust-lang-nursery/rls/issues/722