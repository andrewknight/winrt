# Gerrit Code Review Changes

This section includes references to relevant code changes through the Qt Project's [openly governed code review system](https://codereview.qt-project.org/), as marked with the "g" notation (<sup>g00</sup>).

#### Make specification
1. Knight, Andrew. "Initial mkspec and platform detection of WinRT". December 4, 2012. https://codereview.qt-project.org/#change,39875
1. Wolff, Oliver. "Fixed paths in winrt mkspec". January 9, 2013. https://codereview.qt-project.org/#change,44268
1. Wolff, Oliver. "Update mkspec format for winrt and winphone mkspecs". February 13, 2013. https://codereview.qt-project.org/#change,47035
1. Wolff, Oliver. "Use common base mkspec for winrt and winphone specs". February 13, 2013. https://codereview.qt-project.org/#change,47036
1. Wolff, Oliver. "WinRT plugin: Use correct compile flags and set name". Februrary 28, 2013. https://codereview.qt-project.org/#change,49349
1. Knight, Andrew. "Fix WinRT mkspecs". April 23, 2013. https://codereview.qt-project.org/#change,54531

#### Build system
1. Wolff, Oliver. "Use proper condition instead of duplicate code in qt.prf". February 12, 2013. https://codereview.qt-project.org/#change,47568
1. Trzciński, Kamil. "qmake: added WinRT and WinPhone configuration switch". Feburary 14, 2013. https://codereview.qt-project.org/#change,47559
1. Trzciński, Kamil. "Set path, lib and include for winphone makefile builds". Feburary 14, 2013. https://codereview.qt-project.org/#change,47665
1. Trzciński, Kamil. "qmake: added VCPROJ_ARCH variable". Feburary 14, 2013. https://codereview.qt-project.org/#change,47556
1. Knight, Andrew. "Fix nmake generation for Windows Phone builds". April 22, 2013. https://codereview.qt-project.org/#change,54196
1. Knight, Andrew. "Fix winphone makefile generator". April 24, 2013. https://codereview.qt-project.org/#change,54495

#### Base system
1. Kalinowski, Maruice. "Compile fix for WinRT". November 8, 2012. https://codereview.qt-project.org/#change,39164
1. Kalinowski, Maurice. "WinRT related locale changes". November 8, 2012. https://codereview.qt-project.org/#change,39167, https://codereview.qt-project.org/#change,39165
1. Kalinowski, Maurice. "use successor functions for WinRT". November 8, 2012. https://codereview.qt-project.org/#change,39199
1. Kalinowski, Maurice. "Use standard functions on WinRT". November 19, 2012. https://codereview.qt-project.org/#change,39651
1. Kalinowski, Maurice. "use a static array on WinRT". November 19, 2012. https://codereview.qt-project.org/#change,39653
1. Kalinowski, Maurice. "winrt specific updates". November 19, 2012. https://codereview.qt-project.org/#change,39650
1. Trzciński, Kamil. "Windows RT and Windows Phone preliminary support". February 12, 2013. https://codereview.qt-project.org/#change,46916
1. Wolff, Oliver. "Fixed qunsetenv for WinRT". February 21, 2013. https://codereview.qt-project.org/#change,48560

#### Bootstrap
1. Knight, Andrew. "Updated winmain for use in WinRT". December 4, 2012. https://codereview.qt-project.org/#change,39876
1. Knight, Andrew. "WinRT: Enable command line passing from main". March 21, 2013. https://codereview.qt-project.org/#change,51187

#### JIT Issues
https://codereview.qt-project.org/#change,44268
https://codereview.qt-project.org/#change,53615

#### File handling
1. Kalinowski, Maurice. "WinRT cannot handle library loading outside application bundle". November 12, 2012. https://codereview.qt-project.org/#change,39166
1. Wolff, Oliver. "Add qfilesystemwatcher_win files in "main" win block in io.pri". February 12, 2013. https://codereview.qt-project.org/#change,47569
1. Wolff, Oliver. "Use Windows::ApplicationModel to obtain application path". February 28, 2013. https://codereview.qt-project.org/#change,49345
1. Wolff, Oliver. "Fixed QFileSystemEngine::absoluteName for WinRT". February 28, 2013. https://codereview.qt-project.org/#change,49348
1. Wolff, Oliver. "Make QLockFile compile on WinRT". March 18, 2013. https://codereview.qt-project.org/#change,51110
1. Knight, Andrew. "WinRT: Use relative paths when calling LoadPackagedLibrary". March 27, 2013. https://codereview.qt-project.org/#change,52251
1. Kalinowski, Maurice. "remove starting dir separator". May 8, 2013. https://codereview.qt-project.org/#change,55694

#### Event system
1. Wolff, Oliver. "Separate QEventDispatchers for WinRT and Windows Phone 8". February 14, 2013. https://codereview.qt-project.org/#change,47037
1. Knight, Andrew. "Unify Windows Phone & WinRT Event Dispatchers". April 22, 2013. https://codereview.qt-project.org/#change,54226

#### Networking
1. Wolff, Oliver. "Removed network for winrt non phone builds". February 14, 2013. https://codereview.qt-project.org/#change,47039

#### Platform Abstraction
1. Knight, Andrew. "WinRT QPA plugin". November 5, 2012. https://qt.gitorious.org/~aknight/qt/aknights-qtbase/commit/605f4f91ebc1d48dc5b96b2ec71edca999689a34
1. Trzciński, Kamil. "Windows RT and Windows Phone QPA". February 15, 2013. https://codereview.qt-project.org/#change,46917
1. Wolff, Oliver and Knight, Andrew. March 25, 2013. "WinRT: Do not repaint whole screen on every update". https://codereview.qt-project.org/#change,51578
1. Knight, Andrew. "WinRT: Top-level windows should always be fullscreen". April 19, 2013. https://codereview.qt-project.org/#change,54284
1. Knight, Andrew. "Don't use dirty flip on phone". April 23, 2013. https://codereview.qt-project.org/#change,54547
1. Knight, Andrew. "WinRT: Support screen orientation changes". April 24, 2013. https://codereview.qt-project.org/#change,54575

#### Cursor Handling
1. Knight, Andrew. "WinRT: Implement Platform Cursor". April 22, 2013. https://codereview.qt-project.org/#change,54375
1. Knight, Andrew. "Disable cursor on Windows Phone". April 23, 2013. https://codereview.qt-project.org/#change,54548
1. Knight, Andrew. "WinRT: Refactor pointer handling". April 24, 2013. https://codereview.qt-project.org/#change,54383

#### Keyboard Handling
1. Wolff, Oliver. "WinRT: Added rudimentary key input support". March 15, 2013. https://codereview.qt-project.org/#change,50827
1. Knight, Andrew. "WinRT: Basic Input Context Support". April 22, 2013. https://codereview.qt-project.org/#change,52603
1. Knight, Andrew. "WinRT: Improve key handling". May 19, 2013. https://codereview.qt-project.org/#change,56450

#### Platform Services
1. Knight, Andrew. "WinRT: Introduce Platform Services". April 25, 2013. https://codereview.qt-project.org/#change,54374

#### OpenGL/ANGLE
1. Knight, Andrew. "Upgrade ANGLE to DX11 Proto". April 8, 2013. https://codereview.qt-project.org/#change,52810
1. Knight, Andrew. "Patch ANGLE to support WinRT". April 25, 2013. https://codereview.qt-project.org/#change,51857
1. Knight, Andrew. "Implement OpenGL ES 2 support for WinRT QPA". April 27, 2013. https://codereview.qt-project.org/#change,51858


