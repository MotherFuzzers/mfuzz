# mfuzz experimental implementation based on libFuzzer

```bash
$ git clone https://chromium.googlesource.com/chromium/llvm-project/compiler-rt/lib/fuzzer
<...>

$ git log -n 1
commit 9aedd38104945ebdede9c8c9eeaff105ed9729d0
Author: kcc <kcc>
Date:   Fri Dec 14 23:21:31 2018 +0000

    [libFuzzer] make len_control less aggressive
    
    git-svn-id: https://llvm.org/svn/llvm-project/compiler-rt/trunk/lib/fuzzer@349210 91177308-0d34-0410-b5e6-96231b3b80d8

$ mv fuzzer src
```
