INCLUDE_DIRECTORIES (
  ../engine
  ${PYTHON_INCLUDE_DIRS}
)

SET (PYTHON_SRCS
  PythonSymdiffCommands.cc
)

ADD_LIBRARY (symdiff_py SHARED ${PYTHON_SRCS})

TARGET_LINK_LIBRARIES (symdiff_py symdiff ${PYTHON_LIBRARIES})
IF(WIN32)
SET_TARGET_PROPERTIES(symdiff_py PROPERTIES SUFFIX ".pyd" OUTPUT_NAME "symdiff")
ELSE(WIN32)
SET_TARGET_PROPERTIES(symdiff_py PROPERTIES PREFIX "" SUFFIX ".so" OUTPUT_NAME "symdiff")
ENDIF(WIN32)

#find python site-packages with prefix split off
execute_process ( COMMAND python -c "from distutils.sysconfig import get_python_lib, PREFIX ; print get_python_lib().split(PREFIX)[1]" OUTPUT_VARIABLE PYTHON_SITE_PACKAGES OUTPUT_STRIP_TRAILING_WHITESPACE)

set(PYTHON_INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/${PYTHON_SITE_PACKAGES}/)


install(TARGETS symdiff_py LIBRARY DESTINATION ${PYTHON_INSTALL_DIR})

