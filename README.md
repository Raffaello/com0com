# com0com

This project has been modernized to be compiled through `cmake`.

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

signtool sign /v /fd SHA256 /f com0com.pfx /t "http://timestamp.digicert.com" ..\out\build\x64-Debug\sys\com0com.sys
```

**NOTE:** replace the `out` dir `x64-Debug` with `x64-Release`.

## 


