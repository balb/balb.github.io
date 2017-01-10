---
layout: post
title:  "IIS Express COM Exception"
---

My app was running OK under Local IIS but when I switched to use IIS Express in Visual Studio I started to get this error

```
Exception thrown: 'System.Runtime.InteropServices.COMException' in mscorlib.dll

Additional information: Retrieving the COM class factory for component with CLSID {...} failed due to the following error: 80040154 Class not registered (Exception from HRESULT: 0x80040154 (REGDB_E_CLASSNOTREG)).
```

It turns out that IIS Express runs in 32 bit mode by default. Switching it to 64 bit mode fixed the problem.

In Visual Studio go to Tools > Options > Projects and Solutions > Web Projects > Use the 64 bit version of IIS Express for web sites and projects

Thanks to [ppolyzos](https://ppolyzos.com/2015/12/01/enable-x64-bit-version-of-iis-express/) for this.
