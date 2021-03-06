# Add the unit test library.
# NOTE: probably we should have a check in the build system to determine
# whether Boost libraries are static or dynamic.
ADD_DEFINITIONS(-DBOOST_TEST_DYN_LINK)

MACRO(ADD_DCGP_TESTCASE arg1)
	IF(CMAKE_BUILD_TYPE STREQUAL "Debug")
		ADD_EXECUTABLE(${arg1} ${arg1}.cpp)
		TARGET_LINK_LIBRARIES(${arg1} ${MANDATORY_LIBRARIES} ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY})
        target_compile_options(${arg1} PUBLIC "$<$<CONFIG:DEBUG>:${YACMA_CXX_FLAGS_DEBUG}>")
        target_compile_options(${arg1} PUBLIC "$<$<CONFIG:RELEASE>:${YACMA_CXX_FLAGS}>")
        set_property(TARGET ${arg1} PROPERTY CXX_STANDARD 14)
        set_property(TARGET ${arg1} PROPERTY CXX_STANDARD_REQUIRED YES)
		ADD_TEST(${arg1} ${arg1})
	ENDIF(CMAKE_BUILD_TYPE STREQUAL "Debug")
ENDMACRO(ADD_DCGP_TESTCASE)

MACRO(ADD_DCGP_PERFORMANCE_TESTCASE arg1)
	IF(CMAKE_BUILD_TYPE STREQUAL "Release")
		ADD_EXECUTABLE("${arg1}_perf" "${arg1}_perf.cpp")
		TARGET_LINK_LIBRARIES("${arg1}_perf" ${MANDATORY_LIBRARIES} ${Boost_CHRONO_LIBRARY} ${Boost_TIMER_LIBRARY} ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY} ${Boost_SYSTEM_LIBRARY})
        target_compile_options("${arg1}_perf" PUBLIC "$<$<CONFIG:DEBUG>:${YACMA_CXX_FLAGS_DEBUG}>")
        target_compile_options("${arg1}_perf" PUBLIC "$<$<CONFIG:RELEASE>:${YACMA_CXX_FLAGS}>")
        set_property(TARGET "${arg1}_perf" PROPERTY CXX_STANDARD 14)
        set_property(TARGET "${arg1}_perf" PROPERTY CXX_STANDARD_REQUIRED YES)
		ADD_TEST("${arg1}_perf" "${arg1}_perf")
	ENDIF(CMAKE_BUILD_TYPE STREQUAL "Release")
ENDMACRO(ADD_DCGP_PERFORMANCE_TESTCASE)


ADD_DCGP_TESTCASE(compute)
ADD_DCGP_TESTCASE(mutate)
ADD_DCGP_TESTCASE(differentiate)
ADD_DCGP_TESTCASE(quadratic_error)

ADD_DCGP_PERFORMANCE_TESTCASE(function_calls)
ADD_DCGP_PERFORMANCE_TESTCASE(compute)
ADD_DCGP_PERFORMANCE_TESTCASE(mutate)
ADD_DCGP_PERFORMANCE_TESTCASE(differentiate)
