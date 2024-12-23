cmake_minimum_required(VERSION 3.5)

project(QCamTest VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets OpenGL OpenGLWidgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets OpenGL OpenGLWidgets)

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
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET QCamTest APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(QCamTest SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(QCamTest
            ${PROJECT_SOURCES}
        )
    endif()
endif()

find_package(PkgConfig)
pkg_check_modules(LIBCAMERA REQUIRED IMPORTED_TARGET libcamera)
pkg_check_modules(LIBEVENT REQUIRED IMPORTED_TARGET libevent)
pkg_check_modules(LIBEVENT_THREAD REQUIRED IMPORTED_TARGET libevent_pthreads)

target_compile_definitions(QCamTest PRIVATE QT_NO_KEYWORDS)
target_link_libraries(QCamTest PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::OpenGL Qt${QT_VERSION_MAJOR}::OpenGLWidgets PkgConfig::LIBCAMERA PkgConfig::LIBEVENT PkgConfig::LIBEVENT_THREAD)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.QCamTest)
endif()
set_target_properties(QCamTest PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS QCamTest
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(QCamTest)
endif()
