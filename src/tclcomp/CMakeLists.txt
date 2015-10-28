
INCLUDE_DIRECTORIES (
  ../engine
  ${TCL_INCLUDE}
)

SET (TCL_SOURCES
    TclCommands.cc
    TclSymdiffCommands.cc
    tclcomp.cc
)

ADD_LIBRARY (symdiff_tcl SHARED ${TCL_SOURCES})
TARGET_LINK_LIBRARIES (symdiff_tcl symdiff ${TCL_LIBRARIES})
SET_TARGET_PROPERTIES(symdiff_tcl PROPERTIES OUTPUT_NAME "symdifftcl")

SET (PYTHON_SRCS
  PythonSymdiffCommands.cc
)


#GET_TARGET_PROPERTY(SYMDIFF_TCL_NAME symdiff_tcl LOCATION)
ADD_CUSTOM_COMMAND(TARGET symdiff_tcl
          POST_BUILD
          COMMAND ${TCL_TCLSH} ${CMAKE_SOURCE_DIR}/scripts/mkindex.tcl
	  COMMENT "running TCL mkindex script"
)
#IF (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
#SET_TARGET_PROPERTIES(symdiff_tcl PROPERTIES INSTALL_RPATH "\$ORIGIN/")
#ENDIF (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
#IF (APPLE)
#SET_TARGET_PROPERTIES(symdiff_tcl PROPERTIES INSTALL_RPATH "@loader_path/")
#ENDIF (APPLE)


