# com0com

This project has been modernized to be compiled through `cmake`.

This will support only x64 Windows 10 and later.

For odler system use older versions.

The generator required is `Visual Studio` due to the actual CMake/Ninja not able to compile .NET (are you sure?)



## Pre-requisites

- WDK (Window Driver Kit) to be installed.
- `cmake`
- NSIS

## Self-signing Driver

go in the `certs` dir and run:

```ps

cd cert

makecert -r -n "CN=com0com (test)" -sv com0com.pvk com0com.cer

pvk2pfx -pvk com0com.pvk -spc com0com.cer -pfx com0com.pfx

signtool sign /v /fd SHA256 /f com0com.pfx /t "http://timestamp.digicert.com" ..\out\build\x64-Release\sys\com0com.sys
```


## create .CAT file

Use `inf2cat` utility function. By default is not in the path.
And is located in `C:\Program Files (x86)\Windows Kits\10\bin\10.0.26100.0\x86\Inf2Cat.exe"`

1. Create an `amd64` folder in the output directory
2. Copy into it all the projects outputs:
- `com0com.sys` (signed)
- `setup.lib`
- `setup.dll`
- `setupc.exe`
- `setupg.exe`
3. Copy into it all the `.inf` and `.txt` files from the root repo project:
- `cncport.inf`
- `comport.inf`
- `com0com.inf`
- `Readme.txt`
- `License.txt`


4. Then from the `amd64` folder run:
```ps
"C:\Program Files (x86)\Windows Kits\10\bin\10.0.26100.0\x86\Inf2Cat.exe" /driver:. /os:10_x64 /v

```

5. The utility generate a `com0com.cat` file






