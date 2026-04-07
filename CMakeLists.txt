cmake_minimum_required(VERSION 3.16)

project(dde-store)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_CCX_STANDARD 11)
set(CMAKE_CCX_STANDARD_REQUIRED ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

find_package(PkgConfig REQUIRED)
find_package(Qt6 REQUIRED Core Widgets Network DBus Gui LinguistTools)
find_package(Dtk6 REQUIRED Widget Gui)
find_package(PackageKitQt6 REQUIRED)
find_package(AppStreamQt 1.0 REQUIRED)
find_package(PkgConfig REQUIRED)
include(FindPkgConfig)
include(GNUInstallDirs)

file(GLOB TS_FILES ./translations/*.ts)
qt_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})

file(GLOB SRC
    main.cpp
    mainwindow.cpp
    mainwindow.h
    pages/homepage.cpp
    pages/homepage.h
    pages/itempage.cpp
    pages/itempage.h
    pages/categorypage.cpp
    pages/categorypage.h
    pages/updatespage.cpp
    pages/updatespage.h
    backend/sourcemanager.cpp
    backend/sourcemanager.h
    backend/sources/packagekit/packagekitsource.cpp
    backend/sources/packagekit/packagekitsource.h
    backend/ratingshelper.cpp
    backend/ratingshelper.h
    backend/settings.cpp
    backend/settings.h
    widgets/gallery.cpp
    widgets/gallery.h
    widgets/stars.cpp
    widgets/stars.h
    widgets/list.cpp
    widgets/list.h
    resources/resources.qrc
)

add_executable(${PROJECT_NAME}
    ${SRC}
    ${TS_FILES}
    ${QM_FILES}
)

target_link_libraries(${PROJECT_NAME}
    Dtk6::Widget
    Dtk6::Core
    Dtk6::Gui
    Qt6::Widgets
    Qt6::Network
    PK::packagekitqt6
    AppStreamQt
)

add_subdirectory(backend/sources)

set(CMAKE_INSTALL_PREFIX /usr)

install(TARGETS dde-store DESTINATION bin)
install(FILES ${QM_FILES} DESTINATION share/dde-store/translations)
install(FILES resources/dde-store.desktop DESTINATION share/applications)
