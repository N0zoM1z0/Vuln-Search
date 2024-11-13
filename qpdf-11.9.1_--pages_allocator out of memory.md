# Summary
There is an "allocator is out of memory" vulnerability in qpdf-11.9.1(https://github.com/qpdf/qpdf/releases/tag/v11.9.1).

Affected component is `--pages` option.

# Details
Build from source code with afl-clang-fast and ASAN enabled:
```bash
cmake -DCMAKE_C_COMPILER=afl-clang-fast -DCMAKE_CXX_COMPILER=afl-clang-fast++ -S . -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake -DCMAKE_C_COMPILER=afl-clang-fast -DCMAKE_CXX_COMPILER=afl-clang-fast++ --build build
AFL_USE_ASAN=1 make
```

When use `--pages` options with a crafted pdf file:
```
/home/n0zom1z0/qpdf-11.9.1/build/qpdf/qpdf ./c4 --pages ./c4 1 -- ../../tmp/output.pdf
```

error occurs:
![image](https://github.com/user-attachments/assets/fa948cbb-e939-4b29-9cc8-81c9463c2820)


And I upload this crafted pdf file here: https://github.com/N0zoM1z0/Vuln-Search/blob/main/c4

# PoC
c4: https://github.com/N0zoM1z0/Vuln-Search/blob/main/c4
```
/home/n0zom1z0/qpdf-11.9.1/build/qpdf/qpdf ./c4 --pages ./c4 1 -- ../../tmp/output.pdf
```
![image](https://github.com/user-attachments/assets/fa948cbb-e939-4b29-9cc8-81c9463c2820)
