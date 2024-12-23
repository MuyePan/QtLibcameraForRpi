# Qt Demo Application on RPI with libcamera
This page shows how to build the Qt Demo Application on RPI with libcamera. 

Click the follow image to view this tutorial on Youtube.

## Create an Qt Widgets Application

## Download the source code

## Copy **common** and **qcam** folders into the folder of newly created application

## Remove original files and add new files to the application

## Add pkg config to **CMakeLists.txt**
-find_package(PkgConfig)
-pkg_check_modules(LIBCAMERA REQUIRED IMPORTED_TARGET libcamera)
-pkg_check_modules(LIBEVENT REQUIRED IMPORTED_TARGET libevent)
-pkg_check_modules(LIBEVENT_THREAD REQUIRED IMPORTED_TARGET libevent_pthreads)

## Add **OpenGL OpenGLWidgets** as required packages and link the libraries

## Disable Qt keywords
-target_compile_definitions(QCamTest PRIVATE QT_NO_KEYWORDS)

## Enjoy
