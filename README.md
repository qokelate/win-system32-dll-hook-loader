
# win-system32-dll-hook-loader

## prebuild binary [https://github.com/qokelate/win-system32-dlls.git](https://github.com/qokelate/win-system32-dlls.git)


## usage

* copy dll to exe directory
* put hook dll to exe directory
* rename hook dll to XXX.hook.dll(XXX is name of exe)

## e.g.
* exe file is: ABC.exe (depend winmm.dll)
* copy repo/xARCH/winmm.dll to ABC.exe directory
* rename hook dll to ABC.hook.dll
* start ABC.exe, exe will load winmm.dll => system32/winmm.dll => ABC.hook.dll

## loader code

```
extern "C" BOOL __stdcall DllMain( HMODULE hModule,	DWORD ul_reason_for_call,LPVOID lpReserved)
{
	switch (ul_reason_for_call)
	{
	case DLL_PROCESS_ATTACH:
    {
        module_init();
        wchar_t tmp1[2048];
        GetModuleFileNameW(NULL, tmp1, _countof(tmp1));
        PathRemoveExtensionW(tmp1);
        wcscat(tmp1, L".hook.dll");
        LoadLibraryW(tmp1);
        break;
    }
	case DLL_PROCESS_DETACH:
		break;
	}
	return TRUE;
}
```
