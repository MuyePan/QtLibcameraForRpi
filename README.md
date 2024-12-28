# Qt6 Demo Application Using libcamera on Raspberry Pi
This page shows how to build the Qt6 Demo Application Using libcamera on Raspberry Pi. 

It is highly recommended to watch video tutorial as there is additional content in the video tutorial.

Click the following image to view this tutorial on Youtube.
[![Youtube video link](https://i.ytimg.com/vi/gTd6dm9ONSk/hqdefault.jpg)](//youtu.be/gTd6dm9ONSk "Youtube Video")

- Cross Compilation https://youtu.be/8kpHgNKPooc
- Remote Debugging https://youtu.be/QWz-4R4kMIo

## Create an Qt Widgets Application
![image](https://github.com/user-attachments/assets/3d1fb799-b80b-451b-afd0-d1f6a776da18)

## Download the source code
Download source code from https://github.com/raspberrypi/libcamera. 

Only **src/apps/common** and **src/apps/qcam** folders are needed.

## Copy **common** and **qcam** folders into the folder of newly created application
Ignore dng_writer.h and dng_writer.cpp as we don't define HAVE_TIFF
```
#set(PROJECT_SOURCES
#        main.cpp
#        mainwindow.cpp
#        mainwindow.h
#        mainwindow.ui
#)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(QCamTest
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
        qcam/cam_select_dialog.cpp
        qcam/cam_select_dialog.h
        qcam/format_converter.cpp
        qcam/format_converter.h
        qcam/main.cpp qcam/main_window.cpp
        qcam/main_window.h qcam/meson.build
        qcam/message_handler.cpp
        qcam/message_handler.h
        qcam/viewfinder.h
        qcam/viewfinder_gl.cpp
        qcam/viewfinder_gl.h
        qcam/viewfinder_qt.cpp
        qcam/viewfinder_qt.h
        common/event_loop.cpp
        common/event_loop.h
        common/image.cpp
        common/image.h
        common/meson.build
        common/options.cpp
        common/options.h
        common/ppm_writer.cpp
        common/ppm_writer.h
        common/stream_options.cpp
        common/stream_options.h
        qcam/assets/shader/shaders.qrc
    )
```

## Add pkg to **CMakeLists.txt**
```
find_package(PkgConfig)
pkg_check_modules(LIBCAMERA REQUIRED IMPORTED_TARGET libcamera)
pkg_check_modules(LIBEVENT REQUIRED IMPORTED_TARGET libevent)
pkg_check_modules(LIBEVENT_THREAD REQUIRED IMPORTED_TARGET libevent_pthreads)
```

## Add **OpenGL OpenGLWidgets** as required packages
```
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets OpenGL OpenGLWidgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets OpenGL OpenGLWidgets)
```

## Link the libraries
```
target_link_libraries(QCamTest PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::OpenGL Qt${QT_VERSION_MAJOR}::OpenGLWidgets PkgConfig::LIBCAMERA PkgConfig::LIBEVENT PkgConfig::LIBEVENT_THREAD)
```

## Disable Qt keywords
```
target_compile_definitions(QCamTest PRIVATE QT_NO_KEYWORDS)
```

## Enjoy
![image](https://github.com/user-attachments/assets/d8603a98-a0b4-4448-9aa4-d3149f28b4c3)
