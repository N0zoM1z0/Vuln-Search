# Summary
There is an "allocator is out of memory" vulnerability in qpdf-11.9.1(https://github.com/qpdf/qpdf/releases/tag/v11.9.1).

Affected component is `--rotate` option.

# Details
Build from source code with afl-clang-fast and ASAN enabled:
```bash
cmake -DCMAKE_C_COMPILER=afl-clang-fast -DCMAKE_CXX_COMPILER=afl-clang-fast++ -S . -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake -DCMAKE_C_COMPILER=afl-clang-fast -DCMAKE_CXX_COMPILER=afl-clang-fast++ --build build
AFL_USE_ASAN=1 make
```

When use `--rotate` options with a crafted pdf file:
```
/home/n0zom1z0/qpdf-11.9.1/build/qpdf/qpdf ./c1 --rotate=+90 -- /home/n0zom1z0/tmp/outpu1t.pdf
```

error occurs:
![image](https://github.com/user-attachments/assets/43530161-72a9-4c52-bd47-e07f2feb748c)

And I upload this crafted pdf file here: https://github.com/N0zoM1z0/Vuln-Search/blob/main/c1

# PoC
c1: https://github.com/N0zoM1z0/Vuln-Search/blob/main/c1
```
/home/n0zom1z0/qpdf-11.9.1/build/qpdf/qpdf ./c1 --rotate=+90 -- /home/n0zom1z0/tmp/outpu1t.pdf
```
![image](https://github.com/user-attachments/assets/43530161-72a9-4c52-bd47-e07f2feb748c)
