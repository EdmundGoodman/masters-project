# Installations for the project

## LLVM MLIR

Following the instructions at <https://mlir.llvm.org/getting_started/#unix-like-compiletesting>:

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

On MacOS, requires `brew` installing `ninja` and `cmake`, along with xcode developer tools.

