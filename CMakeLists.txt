project(lab3)
cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_STANDARD 11)


include_directories(libs/json/src)


set(SRC_LIST
        src/shell/parser/tokenization.h
        src/shell/parser/tokenize.cpp

        src/shell/parser/tokens/meta.hpp
        src/shell/parser/tokens/meta.cpp
        src/shell/parser/tokens/AssignmentToken.hpp
        src/shell/parser/tokens/ParenthesisToken.hpp
        src/shell/parser/tokens/NameToken.hpp
        src/shell/parser/tokens/StringLiteralToken.hpp
        src/shell/parser/tokens/NumberLiteralToken.hpp

        src/shell/parser/grammar/expressions.hpp
        src/shell/parser/grammar/Statement.hpp
        src/shell/parser/grammar/FunctionCall.hpp
        src/shell/parser/grammar/Variable.hpp
        src/shell/parser/grammar/StringLiteral.hpp
        src/shell/parser/grammar/NumberLiteral.hpp
        src/shell/parser/grammar/parse.h
        src/shell/parser/grammar/parse.cpp

        src/shell/interpreter/Object.hpp
        src/shell/interpreter/Function.hpp
        src/shell/interpreter/Interpreter.h
        src/shell/interpreter/Interpreter.cpp
        src/shell/interpreter/templ_utils.hpp
        src/shell/interpreter/EmptyObject.cpp
        src/shell/api.h
        src/shell/lib/utils.h
        src/shell/lib/builtins.h
        src/shell/lib/builtins.cpp

        src/app/model/Employee.hpp
        src/app/model/Company.hpp
        src/app/model/Base.hpp
        src/app/model/utils.h
        src/app/shell_api.hpp
        src/app/model/Employee.cpp
        src/app/model/hierarchy.cpp
        src/app/model/hierarchy.h
        src/app/queries.hpp
        src/app/db.hpp
)
set(TESTS_LIST
        tests/shell/tokens/TestAssignmentToken.h
        tests/shell/tokens/TestParenthesisToken.h
        tests/shell/tokens/TestNameToken.h
        tests/shell/tokens/TestNumberLiteralToken.h
        tests/shell/tokens/TestStringLiteralToken.h

        tests/shell/tokens/TestTokensMeta.h
        tests/shell/tokens/TestTokenizing.h

        tests/shell/grammar/TestFunctionCall.h
        tests/shell/grammar/TestTokensParsing.hpp

        tests/utils.h

        tests/shell/TestDynamicTypes.h
        tests/shell/TestShellFunctions.h
        tests/shell/TestObjectToString.h

        tests/app/TestModels.h
        tests/app/TestHierarchyTraversing.h
        tests/app/TestDB.h
)
set(ALL_CODE ${TESTS_LIST} ${SRC_LIST})
add_executable(manage_company src/app/main.cpp ${SRC_LIST})


find_package(CxxTest)
set(CXXTEST_TESTGEN_INTERPRETER bash)
include_directories(${CXXTEST_INCLUDE_DIR})
enable_testing()

FUNCTION(PREPEND var prefix)
    SET(listVar "")
    FOREACH(f ${ARGN})
        LIST(APPEND listVar "${prefix}/${f}")
    ENDFOREACH(f)
    SET(${var} "${listVar}" PARENT_SCOPE)
ENDFUNCTION(PREPEND)

PREPEND(PATHS_FOR_CXXTEST ${CMAKE_CURRENT_SOURCE_DIR} ${ALL_CODE})
CXXTEST_ADD_TEST(tests testrunner.cpp ${PATHS_FOR_CXXTEST})
target_include_directories(tests PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
