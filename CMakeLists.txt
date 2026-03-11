cmake_minimum_required(VERSION 3.15)
project(hello_module)

set(CMAKE_CXX_STANDARD 11)

# Try to locate pybind11 from the active Python environment (e.g. `uv run`).
find_package(Python COMPONENTS Interpreter Development.Module REQUIRED)
execute_process(
    COMMAND "${Python_EXECUTABLE}" -m pybind11 --cmakedir
    OUTPUT_VARIABLE _PYBIND11_CMAKE_DIR
    OUTPUT_STRIP_TRAILING_WHITESPACE
    RESULT_VARIABLE _PYBIND11_CMAKE_DIR_RESULT
)

if(_PYBIND11_CMAKE_DIR_RESULT EQUAL 0 AND EXISTS "${_PYBIND11_CMAKE_DIR}")
    list(APPEND CMAKE_PREFIX_PATH "${_PYBIND11_CMAKE_DIR}")
endif()

find_package(pybind11 CONFIG REQUIRED)

pybind11_add_module(hello
    src/hello.cpp
    src/bindings.cpp
)

install(TARGETS hello DESTINATION .)
