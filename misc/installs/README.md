# Installations for the project

## LLVM MLIR

LLVM MLIR can be installed on MacOS as follows.

### Build dependencies

On MacOS, building MLIR requires installing `ninja` and `cmake`, along with xcode's developer tools.

```bash
xcode-select --install
brew install ninja cmake
```

### Build commands

The following commands modified from <https://mlir.llvm.org/getting_started/#unix-like-compiletesting>
can then be run to build MLIR. The changes include only shallow copying the LLVM repo and including
the commented out clang flags for CMake.

```bash
git clone --depth=1 https://github.com/llvm/llvm-project.git
mkdir llvm-project/build
cd llvm-project/build
cmake -G Ninja ../llvm \
   -DLLVM_ENABLE_PROJECTS=mlir \
   -DLLVM_BUILD_EXAMPLES=ON \
   -DLLVM_TARGETS_TO_BUILD="Native;NVPTX;AMDGPU" \
   -DCMAKE_BUILD_TYPE=Release \
   -DLLVM_ENABLE_ASSERTIONS=ON \
   -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
cmake --build . --target check-mlir
```

### Including built binaries in the PATH

Then, the built binaries can be moved to `~/.local/share` and symlinked to `~/.local/bin` to
be included in the users `PATH` with:

```bash
mv bin ~/.local/share/mlir
ln -s $HOME/.local/share/mlir/mlir-opt mlir-opt # + whatever other tools are being used
```

### Testing the installation

And you can test if this has worked with

```bash
mlir-opt --help
```

### Cleaning up

Finally, the `llvm-project/` directory can be deleted.

