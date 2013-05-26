# Qt as an SDK: a Holistic Approach

Apart from the Qt libraries, an integrated development environment ([IDE](/appendix/terms.md#ide)), known as Qt Creator, is also an important part of the Qt Project. Qt Creator is a desktop application designed to aid in the development and debugging of Qt applications (although it can also be used as an IDE for any general software project). Its main strengths include the ability to manage a Qt project file (.pro file), invoke the corresponding tools (such as [qmake](/appendix/terms.md#qmake)), and launch interactive debugging sessions. It also has excellent code highlighting, navigation, and auto-completion facilities, particularly with QML code. While Qt can of course be used with the platform IDE, it aims to offer (and succeeds in offering[]) better productivity for the development of applications which are heavily Qt-based.

In order to fully support a Qt workflow and developer experience, the tooling must also be adjusted. As such, two main contributions were made in order to solve the problem of WinRT development within the Qt environment:
- plugin for Qt Creator to aid in the deployment and debugging for applications on the WinRT platforms
- a deployment tool, winrtdeployqt, to aid in the packaging and installation of applications using Qt for WinRT


## Qt as a Toolkit
Even with the non-specific styling approach that Qt Quick offers, it is clear that developers are still interested in creating platform-styled UIs[][][]. For that reason, the [Qt Quick Controls](/appendix/terms.md#qt-quick-controls) add-on library was developed, with the intent of bringing much a widget-like API into Qt Quick, including a [new styling API](/appendix/references.md#qt-quick-styling) to give the flexibility and out-of-the-box platform look-and-feel back into Qt UI development. This brings the advantages of the scene graph and language features together with the traditional convenience of a platform look-and-feel. Existing desktop platform style plugins are reused, while new platforms (like WinRT) should utilize the new QML-based styling API.


In order to support tooling for the new platform, a plugin for Qt Creator (the Qt IDE) was created to aid in launching and debugging WinRT applications. Naturally, if the IDE supports the developer's workflow as well or better than the native tools, there will be an increased uptake and usage of the toolchain. Accordingly, it was imperative that the most important tasks provided by the WinRT native tool set (Visual Studio 2012) be supported in Qt Creator: registering the app, launching the app, and attaching the debugger. This work was encapuslated into a Qt Creator plugin, which has been submitted to the Qt Creator source repository[].

Qt Creator already provides advanced support for code editing and project management, so these existing capabilities needed no additional work. The missing details, however, are...

# Project Creation
As mentioned in chapter XXX[], reusing the qtwinmain library allowed for WinRT applications to require no additional initialization code as compared to Qt applications on other platforms. After a series of patches from the Qt community [], qmake also gained support for generating proper makefiles for the build system. The remaining detail was to provide automatic generation of the "Application Manifest"[] and allow it to be registered from the IDE.

# Application Registration and Launch
The existing UI mechanism for launching local applications in Qt Creator is through the Application Launcher<>. The default launcher requires a path to the executable and allows the user to change arguments, the working directory, and optionally run the application within a terminal. As WinRT does not allow direct instatiation of the executable, the executable path and working directory were naturally marked as disabled (though with the real paths still marked within the existing UI). Arguments were left available (as there are useful in testing) as the debugger allows for arguments to be passed to the application (even though there is no way to do this through an installed application's shortcut).

In order to launch the app, it must be registered with the Appx package system, so some extra elements were added to the form in order to ease this process. Following the UI precedent set by the "shadow build" feature[] of Qt Creator, the launch path is highlighted in red if the system detects that the application has not been registered, and displays a message warning the user that the app is not registered, and offering a text link to "Register the app". Upon clicking the link, the text is temporarily grayed-out (disabled) and the gerneral messages window is brought into focus. Qt Creator invokes the proper PowerShell "cmdlet" to register the application, and then checks the output to make sure the application is registered successfully.

After successful registration (and in all cases where the application is detected as already registered), the registration text changes to a new message containing some of the registration details of the application (e.g. application unique ID). Furthermore, a text link appears allowing the user to unregister the app; activating the link performs the converse of the task above: the output window appears and the output of the unregistration script is shown. Upon successful unregistration, the registration text in the launcher UI is reverted to the state described in the previous paragraph.<>

If an error condition is recognized in either step (registration/unregistration), the launcher UI does not change (apart from moving from temporarily disabled to enabled). Rather, the typical error message notification appears in the output window as specified by the Qt Creator UI style guide []. The user is able to see the output of the registration script and is met with appropriate indicators from the application's user interface.<>

A reference of the required PowerShell commands to perform registration and unregistration, as well as the registry keys used to determine registration status, are available in appendix XXX [].


# Application Debugging
The Windows launcher API can be used to start, stop, suspend, and awake Appx packages[]. Upon launch, the process identifier (PID) of the app is passed back to the launcher. While a separate API exists for attaching to debug events[], it is also possible to attach a stand-alone debugger using the PID. In the early stages of the project, the first option was used in order to easily obtain application debug output. As the project evolved, it was clear that the standalone debugger would be much more practical, as all of the code for this was already written (and integrated with Qt Creator) via the CDB plugin. For non-debug runs (launching the app in "normal" mode), the first version is used through the existing OutputDebugString thread instance. Some modification was made in order to receive the events for the current PID, and this was upstreamed to the Qt Creator source repository[] prior to the commit of the rest of the plugin.

When the user launches the application in debug mode, the CDB debugger is attached to the running process the same as a normal Windows application. To acheive this, the Qt Creator plugin added hooks into the existing CDB plugin. Using this, the user can set breakpoints and examine the threads, call stack, variables, and other features provided by the Qt Creator debug mode.

# Appx Manifest
XQuery - xmlpatterns/xmlvalidator

# Application Packaging

In addition to Visual Studio, Microsoft provides both a command-line tool (makeappx.exe) for packaging apps and a C API []. Like the other integration points in Qt Creator, the invokation of command-line tools tend to be the simplest to implement and most in-line with the spirit and utility of qmake. So, like the Appx Manifest generator, this tooling was completed using a qmake feature that invokes makeappx.exe.<>

Because Qt libraries must be packaged with the application (as well as other files marked for deployment), the feature generates a directory file which contains absolute paths to all of the needed DLLs, plugins, and resources such as images. Before passing this file to makeappx.exe, all files are copied to the build directory, which has the added value that the build directory can also be registered in-place.

## Signing
...


## Debugging Insights
- Local
- Remote
- CDB vs ARM debugger

Content..h3.
 2013-04-04 - An
drew Knight - De
bugging insights
..h4. Local..Loc
al debugging wit
h the Qt Creator
 WinRT plugin is
 already usable
because CDB atta
ches to the proc
ess fine using t
he existing CDB
engine. I did no
tice that you mu
st change the de
bugger to x86 wh
en debugging 32-
bit apps or it d
oesn&#39;t work
(the default sel
ected CDB is the
 64-bit one). An
other issue is t
hat the app can
get pretty far b
efore the debugg
er attaches, so
you can use the
following code t
o &quot;wait for
 the debugger&qu
ot;:http://msdn.
microsoft.com/en
-us/library/wind
ows/desktop/ms68
0345(v=vs.85).as
px :..@while (!I
sDebuggerPresent
()) ; // wait@..
Another potentia
l solution to th
e problem is to
instruct the app
lication to laun
ch a CDB debuggi
ng server when t
he app starts. T
his can be done
using &quot;IPac
kageDebugSetting
s&quot;:http://m
sdn.microsoft.co
m/en-us/library/
hh438393.aspx .
 The WinRT Qt Cr
eator plugin wou
ld just need to
instruct CDB to
attach to the re
mote server and
everything shoul
d be the same (u
ntested). This s
etting is persis
tent, so it woul
d need to be cle
ared after every
 debugging sessi
on (or the debug
ging server will
 launch every ti
me the app is la
unched)...h4. Re
mote..&quot;Stan
dard&quot; remot
e debugging on a
ll WinRT platfor
ms is done throu
gh msvsmon (avai
lable in the &qu
ot;Visual Studio
 Windows 8 Remot
e Tools&quot;:ht
tp://msdn.micros
oft.com/en-us/li
brary/hh441469.a
spx#BKMK_Install
). It is a WSDL
service that run
s on the remote
device which rel
ays all the debu
gging data to Vi
sual Studio usin
g Visual Studio
managed data typ
es (original res
earch based on i
nspecting the ne
twork packets).
It runs on port
8016, and appear
s to be running
on the phone whe
n connected via
USB and the scre
en is unlocked (
you can see this
 by running C:\P
rogram Files (x8
6)\Common Files\
Microsoft Shared
\Phone Tools\Cor
eCon\11.0\Bin\Ip
OverUsbEnum.exe)
. For the emulat
or, it seems it
does not start u
ntil Visual Stud
io has instructe
d it to do so, p
resumably by lau
nching msvsmon t
hrough the &quot
;Add-on Package
API&quot;:http:/
/msdn.microsoft.
com/en-us/librar
y/bb513877.aspx
. In other words
, the only way Q
t Creator would
be able to do us
e these debugger
s would be to fi
gure out how to
speak the langua
ge of msvsmon, w
hich would likel
y require using
Visual Studio as
semblies and be
both hackish and
 have questionab
le legal status.
..Interestingly,
 the phone state
s that it is run
ning a separate
debugging servic
e on localhost:8
888. I was not a
ble to connect t
o this port usin
g CDB, so I&#39;
m not sure what
type of service
this is. While I
 did not get aro
und to sniffing
the localhost tr
affic (the USB d
evice is bound t
o the loopback,
not a private IP
 like XDE is), i
t was clear from
 the VS-XDE traf
fic that there i
s no separate de
bugger for XDE;
all traffic come
s from msvsmon.
Assuming we can&
#39;t (or don&#3
9;t want to) use
 msvsmon, we nee
d to use (or bui
ld) a different
debug helper/ser
ver...h4. Remote
 debugging on x8
6/X64..Since CDB
 runs on the des
ktop platforms,
it should be pos
sible to remote
debug like desri
bed in the &quot
;Local&quot; sec
tion. Qt Creator
 already support
s connecting to
a remote CDB ses
sion, so there w
ould just need t
o be a helper ap
plication on the
 remote client t
o perform the ap
plication launch
 and initial att
achment (using t
he same APIs alr
eady used for lo
cal Appx debuggi
ng in Qt Creator
)...h4. Remote d
ebugging on ARM,
 XDE (Emulator),
 and Windows Pho
ne..Since these
platforms do not
 have CDB, IPack
ageDebugSettings
, or the Package
Manager API, the
y need to use a
different mechan
ism. The &quot;C
oreConnectivity
API&quot;:http:/
/msdn.microsoft.
com/en-us/librar
y/bb545992.aspx
is available on
at least XDE and
 Phone (unsure a
bout WinRT ARM d
evices as I don&
#39;t have one t
o test). It is a
n RPC service wh
ich appears to r
un on port 6791
and is used by V
isual Studio to
communicate with
 the emulator or
 physical device
. There are seve
ral .NET assembl
ies (Microsoft.S
martDevice.*) wi
th a high-level
API that do most
 of the work for
 you. These asse
mblies are inclu
ded with the Win
dows Phone 8 SDK
, or more specif
ically the &quot
;Windows 8 Phone
 SDK 8.0 Assembl
ies&quot; packag
e available on t
he Windows Phone
 8 ISO under pac
kages/MobileTool
s/wpsdkcore. Whi
le documentation
 for the API has
n&#39;t been upd
ated since VS200
8, usage example
s are available
from &quot;Windo
ws Phone Power T
ools&quot;:https
://wptools.codep
lex.com/ and thi
s &quot;SO post&
quot;:http://sta
ckoverflow.com/q
uestions/1342073
3/connect-to-win
dows-phone-8-usi
ng-console-appli
cation, or simpl
y by using a .NE
T reflector to i
nspect the assem
blies...I manage
d to build a sim
ple app using th
ese APIs (C++/CL
I so the managed
 portions can be
 compiled separa
tely) which was
able to enumerat
e the devices an
d launch XDE ins
tances. It shoul
d also be possib
le to install/up
date/uninstall a
pps as well as l
aunch/terminate.
 Unfortunately,
I don&#39;t see
many options for
 debugging. It s
eems it may be p
ossible to use t
he Add-on packag
e API to launch
a non-packaged a
pplication (i.e.
 a debugger) tha
t runs with priv
ileges that allo
w access to the
network and the
running applicat
ions...h4. Summa
ry..(Remote) deb
ugging with CDB
on WinRT desktop
 platforms shoul
d just require l
aunching CDB as
a server and hav
ing Qt Creator c
onnect to it (th
is may require a
 helper applicat
ion on the remot
e device to actu
ally launch the
app). For ARM, P
hone, and XDE (w
hich is actually
 an x86 virtual
machine), it see
ms we need to fi
nd another route
, as the standar
d debugger is ms
vsmon which is d
esigned to work
with Visual Stud
io only....



