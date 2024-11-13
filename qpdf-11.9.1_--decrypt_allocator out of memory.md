# Summary
There is an "allocator is out of memory" vulnerability in qpdf-11.9.1(https://github.com/qpdf/qpdf/releases/tag/v11.9.1).

Affected component is `--decrypt` option.

# Details
Build from source code with afl-clang-fast and ASAN enabled:
```bash
cmake -DCMAKE_C_COMPILER=afl-clang-fast -DCMAKE_CXX_COMPILER=afl-clang-fast++ -S . -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake -DCMAKE_C_COMPILER=afl-clang-fast -DCMAKE_CXX_COMPILER=afl-clang-fast++ --build build
AFL_USE_ASAN=1 make
```

When use `--decrypt` options with a crafted pdf file:
```
/home/n0zom1z0/qpdf-11.9.1/build/qpdf/qpdf ./c8 --decrypt -- /home/n0zom1z0/tmp/output/1234.pdf
```

error occurs:
![image](https://github.com/user-attachments/assets/7472c942-3e60-4a8e-b2df-2ed2a0b65402)



And I upload this crafted pdf file here: https://github.com/N0zoM1z0/Vuln-Search/blob/main/c8

# PoC
c8: https://github.com/N0zoM1z0/Vuln-Search/blob/main/c8
```
/home/n0zom1z0/qpdf-11.9.1/build/qpdf/qpdf ./c8 --decrypt -- /home/n0zom1z0/tmp/output/1234.pdf
```
![image](https://github.com/user-attachments/assets/7472c942-3e60-4a8e-b2df-2ed2a0b65402)
