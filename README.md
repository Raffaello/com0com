# com0com

This project has been modernized to be compiled through `cmake`.

This will support only x64 Windows 10 and later.

For odler system use older versions.

The generator required is `Visual Studio` due to the actual CMake/Ninja not able to compile .NET (are you sure?)

# install

before run the setup you must install the self-signed root certificate or disable to check signed driver on windows.
to install import through `crtmgr` the file `com0com.cer` in `cert/` folder.
or just run from `cert/` folder as admin:
```ps
certutil -addstore root .\com0com.cer
```

Then run the `setup`

Otherwise build for the source followin the next paragraph.

# Building

Follow these step for building

## Pre-requisites

- WDK (Window Driver Kit) to be installed.
- `cmake`
- NSIS

## Self-signing Driver

go in the `certs` dir and run:

```ps

cd cert

# makecert -r -n "CN=com0com(test)" -sv com0com.pvk com0com.cer
# pvk2pfx -pvk com0com.pvk -spc com0com.cer -pfx com0com.pfx
# signtool sign /v /fd SHA256 /t "http://timestamp.digicert.com" /f .\com0com.pfx  ..\out\build\x64-Release\sys\Release\com0com.sys

makecert -a sha256 -r -pe -ss PrivateCertStore -n CN="com0com(test)" -eku 1.3.6.1.5.5.7.3.3 com0com.cer 
signtool sign /v /fd SHA256 /s PrivateCertStore /n "com0com(test)" /t "http://timestamp.digicert.com"  ..\out\build\x64-Release\sys\Release\com0com.sys
```


 Then add the self-signed certificate to the certifcate manager in to the trusted root certificates:
```ps
CertMgr /add com0com.cer /s /r localMachine root
```
ref: https://learn.microsoft.com/en-us/windows-hardware/drivers/install/makecert-test-certificate#installing-a-makecert-test-certificate

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
- `license.txt`


4. Then from the `amd64` folder run:
```ps
. "C:\Program Files (x86)\Windows Kits\10\bin\10.0.26100.0\x86\Inf2Cat.exe" /driver:. /os:10_x64 /v
```

5. The utility generate a `com0com.cat` file

6. sign the `com0com.cat`:
```ps
# signtool sign /v /fd SHA256 /t "http://timestamp.digicert.com" /f .\com0com.pfx  ..\out\amd64\com0com.cat
signtool sign /v /fd SHA256 /t "http://timestamp.digicert.com" /s PrivateCertStore /n "com0com(test)"  ..\out\amd64\com0com.cat
```

7. Verify the signature:
```ps
Signtool verify /pa /v /c com0com.cat com0com.inf
```

## Troubleshooting:

> A certificate chain processed, but terminated in a root certificate
- https://learn.microsoft.com/en-us/troubleshoot/windows-server/certificates-and-public-key-infrastructure-pki/valid-root-ca-certificates-untrusted

> must be registered in windows hardware development program
- https://learn.microsoft.com/en-us/windows-hardware/drivers/dashboard/hardware-submission-create
