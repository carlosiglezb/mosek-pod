if(BUILD_TESTING)
  find_package(Threads MODULE REQUIRED)

  add_library(mosek SHARED IMPORTED)
  set_target_properties(mosek PROPERTIES
    INTERFACE_LINK_LIBRARIES
      "${LIBM_LIBRARY};Threads::Threads"
    IMPORTED_LOCATION "${MOSEK_PLATFORM_DIR}/bin/${MOSEK_LIBRARY_FILE}"
    INTERFACE_INCLUDE_DIRECTORIES "${MOSEK_PLATFORM_DIR}/h"
  )

  set(MOSEK_EXAMPLES_DIR
    "${MOSEK_ARCHIVE_DIR}/${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}/tools/examples"
  )

  function(mosek_c_add_test name)
    add_executable(${name}_c "${MOSEK_EXAMPLES_DIR}/c/${name}.c")
    target_link_libraries(${name}_c PRIVATE mosek)
    add_test(NAME ${name}_c COMMAND ${name}_c ${ARGN})
    set_tests_properties(${name}_c PROPERTIES
      LABELS "c;small"
      REQUIRED_FILES "$ENV{HOME}/mosek/mosek.lic"
      TIMEOUT 60
    )
  endfunction()

  mosek_c_add_test(callback intpnt "${MOSEK_EXAMPLES_DIR}/data/25fv47.mps")

  if(CMAKE_C_COMPILER_ID MATCHES Clang OR CMAKE_C_COMPILER_ID STREQUAL GNU)
    target_compile_options(callback_c PRIVATE -Wno-switch)
  endif()

  mosek_c_add_test(cqo1)
  mosek_c_add_test(feasrepairex1 "${MOSEK_EXAMPLES_DIR}/data/feasrepair.lp")
  mosek_c_add_test(lo1)
  mosek_c_add_test(lo2)
  mosek_c_add_test(milo1)
  mosek_c_add_test(mioinitsol)
  mosek_c_add_test(qcqo1)
  mosek_c_add_test(qo1)
  mosek_c_add_test(sensitivity)
  mosek_c_add_test(sdo1)
  mosek_c_add_test(solutionquality)
  mosek_c_add_test(solvebasis)
  mosek_c_add_test(solvelinear)


endif()
