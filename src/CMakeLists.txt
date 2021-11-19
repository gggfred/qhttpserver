cmake_minimum_required(VERSION 3.10)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 COMPONENTS Core Network REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Core Network REQUIRED)

set(QHTTPSERVER_BASE ..)

# Introduce variables:
#   - CMAKE_INSTALL_LIBDIR
#   - CMAKE_INSTALL_BINDIR
#   - CMAKE_INSTALL_INCLUDEDIR
include(GNUInstallDirs)

include_directories(${QHTTPSERVER_BASE}/3rdParty/http-parser)

set(HEADERS_PRIVATE
    ${QHTTPSERVER_BASE}/3rdParty/http-parser/http_parser.h
     qhttpconnection.h
)

set(HEADERS_PUBLIC
    qhttpserver.h
    qhttprequest.h
    qhttpresponse.h
    qhttpserverapi.h
    qhttpserverfwd.h
)

set(SOURCES 
    qhttpconnection.cpp
    qhttprequest.cpp
    qhttpresponse.cpp
    qhttpserver.cpp
    ${QHTTPSERVER_BASE}/3rdParty/http-parser/http_parser.c
) 

add_library(qhttpserver SHARED
    ${SOURCES}
    ${HEADERS_PUBLIC}
    ${HEADERS_PRIVATE}
)
target_link_libraries(qhttpserver Qt${QT_VERSION_MAJOR}::Core Qt${QT_VERSION_MAJOR}::Network)

install(
    TARGETS qhttpserver
    LIBRARY DESTINATION  "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION  "${CMAKE_INSTALL_LIBDIR}"
    RUNTIME DESTINATION  "${CMAKE_INSTALL_BINDIR}"
    INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
)

set(headers.path include)
set(headers.files ${HEADERS_PUBLIC})

foreach(items IN ITEMS headers)
    install(
        FILES ${CMAKE_CURRENT_SOURCE_DIR}/${${items}.files}
        DESTINATION ${${items}.path}
        PERMISSIONS OWNER_READ OWNER_WRITE GROUP_READ WORLD_READ
    )
endforeach()