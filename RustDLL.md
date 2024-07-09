# Fix Missing DLL files for Rust on Windows

## Install msys2 if not already installed
```powershell
winget install MSYS2.MSYS2
```

## Find where you exe is located:
Usually in your projects directory (either target/debug or target/release):
```
C:.
├───migrations
├───src
├───ssl
└───target
    ├───debug        
    │   └───YourProject.exe
    └───release        
        └───YourProject.exe
```

## Open msys2 
You can do this in your current terminal by running:
```powershell
msys2_shell -defterm -here -no-start -msys
```

## Run `ldd` on your executable:
```bash
$ ldd target/release/RESTful.exe
        ntdll.dll => /c/WINDOWS/SYSTEM32/ntdll.dll (0x7ffdc2ff0000)
        KERNEL32.DLL => /c/WINDOWS/System32/KERNEL32.DLL (0x7ffdc2530000)
        KERNELBASE.dll => /c/WINDOWS/System32/KERNELBASE.dll (0x7ffdc0a00000)
        apphelp.dll => /c/WINDOWS/SYSTEM32/apphelp.dll (0x7ffdbcb80000)
        ADVAPI32.dll => /c/WINDOWS/System32/ADVAPI32.dll (0x7ffdc1c20000)
        msvcrt.dll => /c/WINDOWS/System32/msvcrt.dll (0x7ffdc1e10000)
        sechost.dll => /c/WINDOWS/System32/sechost.dll (0x7ffdc2e80000)
        RPCRT4.dll => /c/WINDOWS/System32/RPCRT4.dll (0x7ffdc27f0000)
        bcrypt.dll => /c/WINDOWS/System32/bcrypt.dll (0x7ffdc06b0000)
        bcryptprimitives.dll => /c/WINDOWS/System32/bcryptprimitives.dll (0x7ffdc06e0000)
        ws2_32.dll => /c/WINDOWS/System32/ws2_32.dll (0x7ffdc2780000)
        ucrtbase.dll => /c/WINDOWS/System32/ucrtbase.dll (0x7ffdc0890000)
        secur32.dll => /c/WINDOWS/SYSTEM32/secur32.dll (0x7ffdb1cf0000)
        MSVCP140.dll => /c/WINDOWS/SYSTEM32/MSVCP140.dll (0x7ffd9d360000)
        VCRUNTIME140.dll => /c/WINDOWS/SYSTEM32/VCRUNTIME140.dll (0x7ffda6090000)
        VCRUNTIME140_1.dll => /c/WINDOWS/SYSTEM32/VCRUNTIME140_1.dll (0x7ffda48c0000)
        libssl-3-x64.dll => /mingw64/bin/libssl-3-x64.dll (0x7ffd338f0000)
        libcrypto-3-x64.dll => /mingw64/bin/libcrypto-3-x64.dll (0x7ffd129a0000)
        CRYPT32.dll => /c/WINDOWS/System32/CRYPT32.dll (0x7ffdc0d00000)
        USER32.dll => /c/WINDOWS/System32/USER32.dll (0x7ffdc1ec0000)
        win32u.dll => /c/WINDOWS/System32/win32u.dll (0x7ffdc0f50000)
        GDI32.dll => /c/WINDOWS/System32/GDI32.dll (0x7ffdc2220000)
        gdi32full.dll => /c/WINDOWS/System32/gdi32full.dll (0x7ffdc0770000)
        CRYPTBASE.DLL => /c/WINDOWS/SYSTEM32/CRYPTBASE.DLL (0x7ffdbfc20000)
        msvcp_win.dll => /c/WINDOWS/System32/msvcp_win.dll (0x7ffdc0eb0000)
        SSPICLI.DLL => /c/WINDOWS/SYSTEM32/SSPICLI.DLL (0x7ffdc0580000)
```

## Resolve the issue
Anything not in you're normal windows path you should move to the folder release/debug folder.
So for me I had to move libssl-3-x64.dll and libcrypto-3-x64.dll to that folder:
```
C:.
├───migrations
├───src
├───ssl
└───target
    ├───debug        
    │   ├───YourProject.exe
    │   ├───libssl-3-x64.dll
    │   └───libcrypto-3-x64.dll
    └───release        
        └───YourProject.exe
        ├───libssl-3-x64.dll
        └───libcrypto-3-x64.dll
```

Furthermore, if anything says => not fount, you need to install it:
```bash
$ ldd target/release/RESTful.exe
        libssl-3-x64.dll => not found
        api-ms-win-core-synch-l1-2-0.dll => not found
        api-ms-win-crt-string-l1-1-0.dll => not found
        api-ms-win-crt-math-l1-1-0.dll => not found
        api-ms-win-crt-runtime-l1-1-0.dll => not found
        api-ms-win-crt-stdio-l1-1-0.dll => not found
        api-ms-win-crt-heap-l1-1-0.dll => not found
        api-ms-win-crt-convert-l1-1-0.dll => not found
        api-ms-win-crt-environment-l1-1-0.dll => not found
        api-ms-win-crt-time-l1-1-0.dll => not found
        api-ms-win-crt-utility-l1-1-0.dll => not found
        api-ms-win-crt-filesystem-l1-1-0.dll => not found
        api-ms-win-crt-locale-l1-1-0.dll => not found
```
In this instance I would need to install the package for libssl-3-x64
