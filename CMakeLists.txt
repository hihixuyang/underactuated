cmake_minimum_required(VERSION 3.5)
project(underactuated LANGUAGES NONE)

if(CMAKE_VERSION VERSION_LESS 3.7)
  set(CMAKE_MODULE_PATH
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules/3.7"
    ${CMAKE_MODULE_PATH})
endif()

include(CTest)

find_package(PythonInterp 2.7 REQUIRED)

set(UNDERACTUATED_EXTRACT_EXAMPLES_INPUT_FILE
  "${CMAKE_CURRENT_SOURCE_DIR}/underactuated.html")
set(UNDERACTUATED_EXTRACT_EXAMPLES_OUTPUT_DIR
  "${CMAKE_CURRENT_BINARY_DIR}/examples")
set(UNDERACTUATED_EXTRACT_EXAMPLES_LOG_FILE
  "${CMAKE_CURRENT_BINARY_DIR}/extract_examples.log")

execute_process(
  COMMAND "${PYTHON_EXECUTABLE}" "${CMAKE_CURRENT_SOURCE_DIR}/extract_examples.py" "${UNDERACTUATED_EXTRACT_EXAMPLES_INPUT_FILE}" "${UNDERACTUATED_EXTRACT_EXAMPLES_OUTPUT_DIR}"
  OUTPUT_FILE "${UNDERACTUATED_EXTRACT_EXAMPLES_LOG_FILE}"
  ERROR_FILE "${UNDERACTUATED_EXTRACT_EXAMPLES_LOG_FILE}"
)

if(BUILD_TESTING)
  find_package(Matlab COMPONENTS MAIN_PROGRAM)

  if(Matlab_FOUND)
    file(GLOB_RECURSE UNDERACTUATED_MATLAB_FILES LIST_DIRECTORIES OFF
      "${UNDERACTUATED_EXTRACT_EXAMPLES_OUTPUT_DIR}/*.m")
    list(SORT UNDERACTUATED_MATLAB_FILES)

    foreach(MATLAB_FILE IN LISTS UNDERACTUATED_MATLAB_FILES)
      # TODO(jamiesnape): Add matlab_add_unit_test calls when tests pass.
    endforeach()
  endif()
endif()
