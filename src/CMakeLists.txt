# ##############################################################################
# OASIS: Open Algebra Software for Inferring Solutions
#
# CMakeLists.txt - OASIS library
# ##############################################################################

# These variables MUST be modified whenever a new source file is added.
set(Oasis_SOURCES
    # cmake-format: sortable
    Add.cpp
    Divide.cpp
    Exponent.cpp
    Expression.cpp
    Imaginary.cpp
    Log.cpp
    Multiply.cpp
    Real.cpp
    Subtract.cpp
    Undefined.cpp
    Variable.cpp)

set(Oasis_HEADERS
    # cmake-format: sortable
    ../include/Oasis/Add.hpp
    ../include/Oasis/BinaryExpression.hpp
    ../include/Oasis/Divide.hpp
    ../include/Oasis/Exponent.hpp
    ../include/Oasis/Expression.hpp
    ../include/Oasis/Imaginary.hpp
    ../include/Oasis/LeafExpression.hpp
    ../include/Oasis/Log.hpp
    ../include/Oasis/Multiply.hpp
    ../include/Oasis/Real.hpp
    ../include/Oasis/Subtract.hpp
    ../include/Oasis/Variable.hpp)

# Adds a library target called "Oasis" to be built from source files.
add_library(Oasis ${Oasis_SOURCES} ${Oasis_HEADERS})

if(MSVC)
    target_compile_options(Oasis PRIVATE /W3 /WX)
else()
    target_compile_options(Oasis PRIVATE -Wall -Wextra -Wpedantic -Werror)
endif()

target_compile_features(Oasis PUBLIC cxx_std_20)
target_include_directories(Oasis PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/../include)
target_link_libraries(Oasis PUBLIC fmt::fmt Eigen3::Eigen Taskflow)

# Adds compiler flags for code coverage. Note that only the Clang compiler is
# currently supported for code coverage.
if(OASIS_BUILD_WITH_COVERAGE)
    target_compile_options(Oasis PUBLIC -fprofile-instr-generate
                                        -fcoverage-mapping)
    target_link_options(Oasis PUBLIC -fprofile-instr-generate)
endif()

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES ${Oasis_SOURCES})
