# UI Details

## Platform Menus and Dialogs
QPA, apart from offering the low-level glue between the platform and Qt, also provides some styling features as well. These features remain agnostic to Qt's GUI component offerings (that is Widgets and Quick) and are mainly used to create native menus and integration points. WinRT has a few of these:
- *Popups*: The Popups API[] is used for displaying message dialogs and context-sensitive menus. QPA has some integration suppport for these, but is unfortunately incomplete in terms of mapping all use cases between WinRT can Qt. For example, it Qt supports embedding arbitrary widgets within dialogs and menus - something which is not supported in WinRT. For common use cases, however, it could be feasible to enable native menus within applications. These experimental features were added[], but not enabled by default.

    <image showing native popup, dialog>

- *Pickers*: One of the native integration points in QPA is the native dialog - implementers can use it to provide a platform-specific version of font, color, or file dialogs. WinRT has support for file picking, though not color or font. Since file location likely will not work outside of the App sandbox, it was important to implement this feature. Qt applications can simply create a QFileDialog, and the correct native FilePicker will be launched. This is provided through the Windows.Storage.Pickers[] API.

    <image of native picker>

- *Native menubar*: An interesting feature of some operating systems is the native menu bar, most notably present on Mac OSX. This is a menu bar which is always present, but may change depending on the application currently in focus. Windows 8 introduces a similar concept known as the "charm bar", an access point to various "charms", or integration points for tasks like searching or settings. To enable access to this feature, the native menu system was integrated with the "Settings" charm[]. This allows the developer to create menu items using the existing QMenuBar API. They simply enable the "useNativeMenu" flag, and their menu items appear in settings instead of a widget.

    <image of native menu>

- *Modern "system tray"*: While Windows 8 has no concept of a system "tray" - or notification area - it does offer several new UI elements which cover the same use case. A notification icon is typically displayed next to the clock on a desktop operating system and conveys status (by e.g. changing the icon) of a running application or service. It may also provide popup messages ("balloons") or have an interactive menu. Qt consolidates these cases into the QSystemTrayIcon, which is abstracted in QPA. In order to provide a useful mapping of these features, the following items were implemented:
 - Show a menu: Badge menu?
 - Set icon: This sets the application "badge".
 - Show a tooltip: A tooltip can be displayed when hovering over the tray icon. This is not supported, as there is no icon to hover over.
 - Show a balloon: This was done through the "Toasts" API.
 - Geometry: This returns the geometry of the icon. As badge icons can be different sizes, this is useful for the application to be aware of when rendering to the icon.

## Qt Widgets
While Qt Widgets aren't active developed[] (Qt Quick being their replacement), they do make for a time-tested set of ready-made components for rapid development of GUIs. With the move to Qt 5 & QPA, widgets tend to work "out of the box" on any platform as long as they use the fully-painted codepaths (i.e. not the platform style plugin). This can be a major advantage on devices where OpenGL - or any platform UI toolkit, for that matter - is unavailable. Widgets are simply raster surfaces, and can be used within the performance constraints of the CPU-bound painting system. This includes the Qt Graphics View, a 2D canvas and foundation for Qt Quick 1. This means that widget-based applications do not face the restrictions of Qt Quick 2 when dealing with embedded (phone/tablet) hardware.

<screenshots of widget apps running on tablet and phone>

As there is no concept of desktop controls (widgets) in the Modern UI, Qt widgets can appear a bit out of place here. While styling them is possible[], it would be a large undertaking with few potential users (assuming Qt Quick is enabled on the platform, of course). Qt Quick Controls, the UI library introduced in Qt 5.1, has its own QML-based styling API which should be used on all new platforms. Even so, at least one Qt Project developer[] has committed to styling Qt Widgets for iOS, signalling that there may be ongoing interest in the use of Qt Widgets (even on mobile operating systems).

In order for native styling to be adopted in Qt Quick Controls, the developer should study the native XAML components carefully in order to construct a set of matching controls based on Qt Quick. Making pixel-perfect renderings is difficult in itself, and becomes even more so when considering all animations and behaviors. Undoubtedly, it will take significant investment from the Qt Project should this styling be attempted. This is, after all, the same basic principle that has been applied with existing Qt style plugins on desktop platforms - the difference being that the implementor has the advantage of using Qt Quick and OpenGL instead of the aging QPainter/QStyle API.

## More challenges await
While not a complete port, building Qt applications on WinRT is now a certain possibility. Enabling ANGLE on more devices, providing offline shader compilation, and providing native styling for Qt Quick Controls will be the largest tasks going forward. Even with these limitations, it is possible to build Qt applications for a variety of use cases, and they can even take advantage of the many integration points that the native platform provides.
