::------------------------------------------
::
:: download gcc from: http://tdm-gcc.tdragon.net/download
:: add "C:\MinGW32\bin" to path variable
:: download and install Python: http://www.python.org/  
:: download and install numpy: http://www.scipy.org/Download -> Official releases are on SourceForge: https://sourceforge.net/projects/numpy/files/

:: hints: http://www.develer.com/oss/GccWinBinaries
:: see also pdf file GccWinBinaries.pdf

::------------------------------------------
::build_meDriver_pyd.cmd

setup.py build --compiler=mingw32

pause

::setup.py install build --compiler=mingw32
::the "install" option will copy the created file meDriver.pyd into the folder:
::C:\Python27\Lib\site-packages and will create a *.egg_info file there too.
::the meDriver.pyd is created in the subfolder \build\lib.win32-2.7

::------------------------------------------
::C:\Dokumente und Einstellungen\<username>\Eigene Dateien\meIDSPythonInerface>setup.py
::install build --compiler=mingw32
::running install
::running build
::running build_ext
::building 'meDriver' extension
::C:\MinGW32\bin\gcc.exe -mno-cygwin -mdll -O -Wall -DME_WINDOWS -I../../drv/osi -
::IC:\Python27\include -IC:\Python27\PC -c meDriverModule.c -o build\temp.win32-2.7\Release\medrivermodule.o
::cc1.exe: error: unrecognized command line option '-mno-cygwin'
::error: command 'gcc' failed with exit status 1

::-- solution -->
::http://stackoverflow.com/questions/6034390/compiling-with-cython-and-mingw-produces-gcc-error-unrecognized-command-line-o
::It sounds like GCC 4.7.0 has finally removed the deprecated -mno-cygwin option, 
::but distutils has not yet caught up with it. 
::Either install a slightly older version of MinGW, or edit distutils\cygwinccompiler.py 
::in your Python directory to remove all instances of -mno-cygwin.

:: --> edited distutils\cygwinccompiler.py and removed the option -mno-cygwin everywhere


::------------------------------------------

::C:\Dokumente und Einstellungen\<username>\Eigene Dateien\meIDSPythonInerface>setup.py
::install build --compiler=mingw32
::running install
::running build
::running build_ext
::building 'meDriver' extension
::C:\MinGW32\bin\gcc.exe -mdll -O -Wall -DME_WINDOWS -I../../drv/osi -IC:\Python27
::\include -IC:\Python27\PC -c meDriverModule.c -o build\temp.win32-2.7\Release\me
::drivermodule.o
::meDriverModule.c:25:33: fatal error: Numeric/arrayobject.h: No such file or directory
::compilation terminated.
::error: command 'gcc' failed with exit status 1

::-- solution -->
:: change the following line in meDriverModule.c
::#include <Numeric/arrayobject.h>
:: to the new location of the include file 
::#include   "C:\Python27\Lib\site-packages\numpy\core\include\numpy\arrayobject.h"

::------------------------------------------

::C:\Dokumente und Einstellungen\<username>\Eigene Dateien\meIDSPythonInerface>setup.py
::install build --compiler=mingw32
::running install
::running build
::running build_ext
::building 'meDriver' extension
::C:\MinGW32\bin\gcc.exe -mdll -O -Wall -DME_WINDOWS -I../../drv/osi -IC:\Python27
::\include -IC:\Python27\PC -c meDriverModule.c -o build\temp.win32-2.7\Release\medrivermodule.o
::writing build\temp.win32-2.7\Release\meDriver.def
::creating build\lib.win32-2.7
::C:\MinGW32\bin\gcc.exe -shared -s build\temp.win32-2.7\Release\medrivermodule.o
::build\temp.win32-2.7\Release\meDriver.def -LC:/WINDOWS/system32 -LC:\Python27\li
::bs -LC:\Python27\PCbuild -lmeIDSmain -lpython27 -lmsvcr90 -o build\lib.win32-2.7
::\meDriver.pyd
::c:/mingw32/bin/../lib/gcc/mingw32/4.6.1/../../../../mingw32/bin/ld.exe: cannot find -lmeIDSmain
::collect2: ld returned 1 exit status
::error: command 'gcc' failed with exit status 1

::-- solution -->
::put the file meIDSmain.lib into the directory
::C:\Python27\libs

::------------------------------------------

::C:\Dokumente und Einstellungen\<username>\Eigene Dateien\meIDSPythonInterface>setup.py
:: build --compiler=mingw32
::running build
::running build_ext
::building 'meDriver' extension
::C:\MinGW32\bin\gcc.exe -mdll -O -Wall -DME_WINDOWS -I../../common -IC:\Python27\
::include -IC:\Python27\PC -c meDriverModule.c -o build\temp.win32-2.7\Release\med
::rivermodule.o
::writing build\temp.win32-2.7\Release\meDriver.def
::C:\MinGW32\bin\gcc.exe -shared -s build\temp.win32-2.7\Release\medrivermodule.o
::build\temp.win32-2.7\Release\meDriver.def -LC:/WINDOWS/system32 -LC:\Python27\li
::bs -LC:\Python27\PCbuild -lmeIDSmain -lpython27 -lmsvcr90 -o build\lib.win32-2.7
::\meDriver.pyd
::Warning: resolving _GetModuleHandleA@4 by linking to _GetModuleHandleA
::Use --enable-stdcall-fixup to disable these warnings
::Use --disable-stdcall-fixup to disable these fixups
::Warning: resolving _GetProcAddress@8 by linking to _GetProcAddress
::Warning: resolving _VirtualQuery@12 by linking to _VirtualQuery
::Warning: resolving _VirtualProtect@16 by linking to _VirtualProtect
::Warning: resolving _EnterCriticalSection@4 by linking to _EnterCriticalSection
::Warning: resolving _TlsGetValue@4 by linking to _TlsGetValue
::Warning: resolving _GetLastError@0 by linking to _GetLastError
::Warning: resolving _LeaveCriticalSection@4 by linking to _LeaveCriticalSection
::Warning: resolving _DeleteCriticalSection@4 by linking to _DeleteCriticalSection
::
::Warning: resolving _InitializeCriticalSection@4 by linking to _InitializeCritica
::lSection

::-- solution -->
::ignored warnings for now

::------------------------------------------


::wrong reference to the meIDSmain.dll:
::rename meDriver.pyd to meDriver.dll and open it in dependency walker
::instead the meDriver.pyd uses meIDSmain.dll it references the meIDSmain_NoRPC.dll
::meIDSmain_NoRPC.dll is found as text in meIDSmain.dll

::-- solution -->
::the meIDSmain.dll is found in C:\WINDOWS\system32
::rename the file, so the gcc compiler can not find it any more
::recreate meDriver.pyd
::now the meIDSmain.dll is referenced

::------------------------------------------



