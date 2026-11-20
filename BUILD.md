# Building babushka

Besides a Rust toolchain, `build.rs` needs two things from the system:

| Needed for | What | Debian/Ubuntu | Fedora | macOS | Windows |
|---|---|---|---|---|---|
| `cc` compiles `src/gpc.c` | C compiler | `build-essential` | `gcc` | Xcode Command Line Tools | MSVC Build Tools |
| `bindgen` generates the GPC bindings | clang + libclang | `clang libclang-dev` | `clang clang-devel` | Xcode Command Line Tools | LLVM (`winget install LLVM.LLVM`), set `LIBCLANG_PATH` |

Without libclang the build stops in `build.rs` with
`Unable to generate bindings: ... 'stddef.h' file not found`.

```bash
cargo build
cargo test
```

No system packages? Build in a throwaway container:

```bash
docker run --rm -v "$PWD":/src:ro rust:1-bookworm bash -c \
  'apt-get update && apt-get install -y clang libclang-dev && cp -r /src /w && cd /w && cargo test'
```
