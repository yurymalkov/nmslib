#
# Non-metric Space Library
#
# Authors: Bilegsaikhan Naidan, Leonid Boytsov.
#
# This code is released under the
# Apache License Version 2.0 http://www.apache.org/licenses/.
#
#

include_directories (${NonMetricSpaceLib_SOURCE_DIR}/include ${NonMetricSpaceLib_SOURCE_DIR}/include/space ${NonMetricSpaceLib_SOURCE_DIR}/include)

file(GLOB TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/test*.cc)

list(REMOVE_ITEM TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/bunit.cc)
list(REMOVE_ITEM TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/test_integr.cc)
list(REMOVE_ITEM TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/bench_distfunc.cc)
list(REMOVE_ITEM TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/bench_projection.cc)
list(REMOVE_ITEM TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/src/report_intr_dim.cc)
list(REMOVE_ITEM TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/test_clust.cc)

add_executable (bunit bunit.cc ${TEST_SRC_FILES})
add_executable (bench_distfunc bench_distfunc.cc)
add_executable (test_integr test_integr.cc)
add_executable (report_intr_dim report_intr_dim.cc)
add_executable (test_clust test_clust.cc)

add_dependencies (bench_distfunc NonMetricSpaceLib)
add_dependencies (test_integr NonMetricSpaceLib)
add_dependencies (bunit NonMetricSpaceLib)
add_dependencies (report_intr_dim NonMetricSpaceLib)
add_dependencies (test_clust NonMetricSpaceLib)

if ((NOT WITHOUT_LSH) AND (NOT WIN32))
  add_executable (bench_projection bench_projection.cc)

  add_dependencies (bunit lshkit)
  add_dependencies (bench_distfunc lshkit)
  add_dependencies (bench_projection NonMetricSpaceLib)

  set(LSHKIT_LIB "lshkit")
  set(GSL_LIB    ${GSL_LIBRARIES})
  set(BOOST_LIB  ${Boost_LIBRARIES})

  target_link_libraries (bench_projection NonMetricSpaceLib ${LSHKIT_LIB} ${GSL_LIB} ${BOOST_LIB})
else()
# NO lshkit for Win32 and MACOSX
  set(LSHKIT_LIB "")
  set(GSL_LIB "")
  set(BOOST_LIB ${Boost_LIBRARIES})
endif()

target_link_libraries (bunit NonMetricSpaceLib ${LSHKIT_LIB} ${GSL_LIB} ${BOOST_LIB})
target_link_libraries (test_integr NonMetricSpaceLib ${LSHKIT_LIB} ${GSL_LIB} ${BOOST_LIB})
target_link_libraries (bench_distfunc NonMetricSpaceLib ${LSHKIT_LIB} ${GSL_LIB} ${BOOST_LIB})
target_link_libraries (test_clust NonMetricSpaceLib ${LSHKIT_LIB} ${GSL_LIB} ${BOOST_LIB})
target_link_libraries (report_intr_dim NonMetricSpaceLib ${LSHKIT_LIB} ${Boost_LIBRARIES} ${GSL_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})

if (CMAKE_BUILD_TYPE STREQUAL "Release")
    set (LIBRARY_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/release/")
    set (EXECUTABLE_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/release/")
else ()
    set (LIBRARY_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/debug/")
    set (EXECUTABLE_OUTPUT_PATH "${PROJECT_SOURCE_DIR}/debug/")
endif ()
