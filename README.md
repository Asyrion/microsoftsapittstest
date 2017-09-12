# Microsoft SAPI TTS Example - How To

## How to Setup Text to Speech (TTS) with the Microsoft SAPI C++
To compile and execute C++ code on kubuntu linux we need some tools and software.

1. Setting up the project 
2. Initialize COM
3. Setting up voices
4. Speak!
5. Modifying Speech

### Setting up the project
While it is possible to write an application from scratch, 
it is easier to start from an existing project. In this case, 
use Visual Studio's application wizard to create a Win32 console application. 
Choose "Hello, world" as the sample when asked during the wizard set up. 
After generating it, open the STDAfx.h file and paste the following code 
after "#include <stdio.h>" but before the "#endif" statement. 
This sets up the additional dependencies SAPI requires.

    ```c++
    #define _ATL_APARTMENT_THREADED
    #include <atlbase.h>
    //You may derive a class from CComModule and use it if you want to override something,
    //but do not change the name of _Module
    extern CComModule _Module;
    #include <atlcom.h>`

Next add the paths to SAPI.h and SAPI.lib files. The paths shown are for a 
standard SAPI SDK install. If the compiler is unable to locate either file, 
or if a nonstandard install was performed, use the new path to the files. 
Change the project settings to reflect the paths. Using the Project->Settings menu item, 
set the SAPI.h path. Click the C/C++ tab and select Preprocessor from the Category 
drop-down list. Enter the following in the "Additional include directories": 
C:\Program Files\Microsoft Speech SDK 5.3\Include.

To set the SAPI.lib path:

1. Select the Link tab from the Same Settings dialog box.
2. Choose Input from the Category drop-down list.
3. Add the following path to the "Additional library path":
   C:\Program Files\Microsoft Speech SDK 5.3\Lib\i386.
4. Also add "sapi.lib" to the "Object/library modules" line. Be sure that the name is separated by a space. 

### Initialize COM
SAPI is a COM-based application, and COM must be initialized both before use and during 
the time SAPI is active. In most cases, this is for the lifetime of the host application. 
The following code (from Listing 2) initializes COM. Of course, the application does not do 
anything beyond initialization, but it does ensure that COM is successfully started.    
    `#include <stdafx.h>
    #include <sapi.h>

    int main(int argc, char* argv[])
    {
        if (FAILED(::CoInitialize(NULL)))
            return FALSE;

        ::CoUninitialize();
        return TRUE;
    }`
    
### Setting up Voices
Once COM is running, the next step is to create the voice. A voice is simply a COM object. 
Additionally, SAPI uses intelligent defaults. During initialization of the object, 
SAPI assigns most values automatically so that the object may be used immediately afterward. 
This represents an important improvement from earlier versions. The defaults are retrieved from 
Speech properties in Control Panel and include such information as the voice (if more than one is 
available on your system), and the language (English, Japanese, etc.). While some defaults are obvious, 
others are not (speaking rate, pitch, etc.). Nevertheless, all defaults may be changed either 
programmatically or in Speech properties in Control Panel.

Setting the pVoice pointer to NULL is not required but is useful for checking errors; this ensures an 
invalid pointer is not reused, or as a reminder that the pointer has already been allocated or deallocated


### Speak!
The actual speaking of the phrase is an equally simple task: one line calling the Speak function. 
When the instance of the voice is no longer needed, you can release the object.

### Modifying Speech
Voices may be modified using a variety of methods. The most direct way is to apply XML commands directly 
to the stream. See the XML TTS Tutorial for more details. In this case, a relative rating of 10 will lower 
the pitch of the voice.

[XML TTS Tutorial](https://msdn.microsoft.com/en-us/library/ms717077(v=vs.85).aspx "XML TTS Tutorial")

[Using Events with TTS (SAPI 5.3)](https://msdn.microsoft.com/en-us/library/ms720165(v=vs.85).aspx)
