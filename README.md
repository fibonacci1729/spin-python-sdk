# spin-python-sdk

This is an experiment to build a Spin Python SDK using CPython, wasi-vfs, and PyO3.

## Prerequisites

- [CPython](https://github.com/python/cpython) build prereqs (e.g. Make, Clang, etc.)
- [Rust](https://rustup.rs/) (including `wasm32-wasi` target)
- [wasi-sdk](https://github.com/WebAssembly/wasi-sdk) v16 or later
- [Wizer](https://github.com/bytecodealliance/wizer) v1.6.0 or later
- [Spin](https://github.com/fermyon/spin)

## Building and Running

First, build CPython for wasm32-wasi:

```
git submodule update --init --recursive
mkdir -p cpython/builddir/wasi
mkdir -p cpython/builddir/build
cd cpython/builddir/build
../../configure --prefix=$(pwd)/install --enable-optimizations
make
cd ../wasi
CONFIG_SITE=../../Tools/wasm/config.site-wasm32-wasi ../../Tools/wasm/wasi-env \
    ../../configure -C --host=wasm32-unknown-wasi --build=$(../../config.guess) \
        --with-build-python=$(pwd)/../build/python --prefix=$(pwd)/install --disable-test-modules
make
make install
cd ../../..
```

Then, build and run this app:

```
spin build
spin up
```
