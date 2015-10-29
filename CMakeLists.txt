cmake_minimum_required(VERSION 3.0)
PROJECT (SYMDIFF)

include(GNUInstallDirs)
include(generate.cmake)

SET (SUBDIRS
    src
    testing
)

# superlu stuff
SITE_NAME(HOSTNAME)
MESSAGE("Hostname: ${HOSTNAME}")
set(SOVERSION 0)
ADD_DEFINITIONS(-DSYMDIFF_VERSION_STRING="\\"0.00\\""  -DSYMDIFF_COPYRIGHT_YEAR="\\"2010\\"")


#c++11 support - newer cmake can do this more elegantly
add_compile_options(-std=c++11)



set(PROJECT_BIN_DIR ${PROJECT_SOURCE_DIR}/bin)
set(PROJECT_SCRIPTS_DIR ${PROJECT_SOURCE_DIR}/scripts)
set(PROJECT_LIBRARY_DIR ${PROJECT_SOURCE_DIR}/lib)
set(PROJECT_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)


find_package(PythonLibs REQUIRED)
find_package(TCL REQUIRED)
find_package(TclStub REQUIRED)

find_program(FLEX flex)
find_program(BISON bison)

IF (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
ADD_DEFINITIONS(-Wall -DTCL_THREADS -DUSE_TCL_STUBS -fvisibility=hidden)
ENDIF (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
IF (APPLE)
ADD_DEFINITIONS(-Wall -DTCL_THREADS -DUSE_TCL_STUBS -fvisibility=hidden)
#SET (CMAKE_INSTALL_NAME_DIR "@rpath")
SET(CMAKE_SKIP_BUILD_RPATH TRUE)
ENDIF(APPLE)
IF (WIN32)
ADD_DEFINITIONS(-DTCL_THREADS -DUSE_TCL_STUBS)
ADD_DEFINITIONS(-DSTATIC_BUILD -D_USE_MATH_DEFINES)
SET (CMAKE_CXX_WARNING_LEVEL 4)
# warning C4251 for dll exports
# warning C4005 concerning macro redefines in stdint.h
# warning C4996: 'isatty': The POSIX name for this item is deprecated
SET (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /fp:strict /wd4251 /wd4005 /wd4996")
SET (CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS} /fp:strict /wd4251 /wd4005 /wd4996")
ELSE (WIN32)
ENDIF (WIN32)

INCLUDE(${SYMDIFF_CONFIG}.cmake OPTIONAL)

ENABLE_TESTING()
FOREACH (SUB ${SUBDIRS})
    ADD_SUBDIRECTORY(${SUB})
ENDFOREACH (SUB ${SUBDIRS})

#pre-written headers
set(HEADERS ${CMAKE_SOURCE_DIR}/src/engine/EngineAPI.hh ${CMAKE_SOURCE_DIR}/src/engine/parseexpr.hh   ${CMAKE_SOURCE_DIR}/src/engine/import.hh)

install(FILES ${HEADERS}
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/symdiff)


