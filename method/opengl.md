# (No) OpenGL for WinRT
One of the more controversial aspects of WinRT is the lack of OpenGL support. Perhaps this should not come to a surprise, as Microsoft has their own advanced graphics API, Direct 3D (of which version 11.1 is supported in WinRT). Historically, vendors have provided video drivers to interface with Windows' own WGL implementation. With WGL removed from WinRT, there is no likely candidate to take its place - that is, apart from OpenGL compatibility layers like (*ANGLE).

## ANGLE
The Almost Native Graphics Layer (ANGLE) is an open-source project primarily sponsored by Google[] to produce an OpenGL ES 2 compliant implementation upon DirectX 9. It was originally introduced to improve graphics acceleration support on Windows computers which shipped with decent Direct3D drivers but poor or buggy OpenGL drivers. For Google, it means better graphics (particularly WebGL) in their browser, Chrome. For other contributors, such as TransGaming, it paves a valuable path to providing cross-platform games and applications.

## Updating ANGLE
With the release of Qt 5, Qt already shipped its own copy of ANGLE for much the same reason as Google Chrome: to better support OpenGL use on aging Windows computers. To support Direct3D 11, though, it was important to first upgrade ANGLE based on the bleeding-egde developments of the ANGLE project. This was done using the "DX11 Proto Milestone 6" branch[], and I provided patches to split the functionality between DirectX9 and DirectX11 so only one codepath would be selected at configure time (the ANGLE approach was to load fallback to DX9 if DX11 loading failed; this would be unnacceptable on WinRT as there is on DX9 support there).

## Patching ANGLE for WinRT
After updating Qt's copy of ANGLE, the next step was to produce proper codepaths to adhere to both WinRT's API as well as the new DirectX 11.1 requirements for the WinRT environment.


### Fixing up old APIs
Much like was involved in for Qt Core, ANGLE required a few updates to move away from Win32 APIs no longer supported in WinRT[]. These include:
 - Thread local storage calles (TLS*) were replaced with usage of the threading attribute, __declspec(thread).
 - LocalAlloc/LocalFree dynamic memory methods were replaced with the newer Heap API methods.
 - Calls to LoadLibrary() were replaced with LoadPackagedLibrary(). In the case of Direct3D 11, they were removed in favor of static initialization of the Direct3D library.


### Fixing ANGLE EGL
(*EGL) is a native platform interface primarily used in conjunction with OpenGL ES, but which can also be used with OpenGL and OpenVG. Much like an OpenGL ES chipset vendor, ANGLE provides is own implementation of EGL for providing access to its own version of the OpenGL ES 2 library. Within EGL, native types (such as window handles) are abstracted into platform-independent EGL types. With these types, platform-independent calls made through the EGL API are directed to platform-dependent PIMPLs. Once EGL has been implemented for a platform, the front-end (that is, EGL) code stays the same across platforms.

In order to implement EGL for WinRT, a few patches to ANGLE needed to be made, starting with the native handle system. In ANGLE, the EGLNativeWindowType is defined as (*HWND) (Win32 window handle), while the EGLNativeDisplayType is defined as HDC (Win32 device context handle). Naturally, these references were defined as different handle types on WinRT: (*ICoreWindow) pointers and int, respectively. For references to EGLNativeDisplayType, the changes were trivial: essentially all display-dependent code was dropped for WinRT, because the concept of display in the CoreApplication framework does not provide any functionality which would be needed in OpenGL. For graphics drivers, this actually a fairly common case: the programmer just initializes the EGL_DEFAULT_DISPLAY type and the platform initializes any screen-specific resources without any further requirements from the application. In the case of WinRT, all required information for display construction is located in the window pointer; no information is required of the display/screen.

In functions requiring the native window handle (e.g. those determining the size of the window), codepaths inserted so that ANGLE EGL could deal with initialization and state tracking where appropriate. One of the biggest changes was done to the initialization of the swap chain, as mentioned in the earlier section on Qt Gui. Code from this page flipper was "transplanted" into the ANGLE swap chain implementation, allowing an swap chain to be initialized from the ICoreWindow pointer instead of the HWND.
