#INCLUDE_DIRECTORIES (
#)

SET (MC_SRCS
    ${CMAKE_CURRENT_BINARY_DIR}/mcParser.cc
    ${CMAKE_CURRENT_BINARY_DIR}/mcScanner.cc
    mcModelCompiler.cc
    mcTest.cc
    EquationFunctions.cc
    parseexpr.cc
    Context.cc
    SubExpr.cc
    ProcessModelOrder.cc
    mcModel.cc
)

SET (CXX_SRCS
    EngineAPI.cc
    EquationObject.cc
    UserFunc.cc
    LogicalFunc.cc
    Add.cc
    Constant.cc
    Exponent.cc
    IfElse.cc
    Log.cc
    Pow.cc
    Product.cc
    Variable.cc
    Functions.cc
    Utility.cc
    ProcessOrderedTable.cc
)

INCLUDE_DIRECTORIES (
    ${CMAKE_CURRENT_SOURCE_DIR}
)

ADD_LIBRARY(symdiff SHARED ${CXX_SRCS} ${MC_SRCS})
#ADD_LIBRARY(symdiff_static STATIC ${CXX_SRCS} ${MC_SRCS})



flex_generate(mcScanner "${CMAKE_CURRENT_BINARY_DIR}/mcParser.hh;mcTest.hh;EquationObject.hh;UserFunc.hh" mc)

bison_generate(mcParser mc) 

set_target_properties(symdiff PROPERTIES SOVERSION ${SOVERSION})

install(TARGETS symdiff LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

#install generated headers
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/mcParser.hh DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/symdiff)
