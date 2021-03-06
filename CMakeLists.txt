cmake_minimum_required(VERSION 2.8.3)
project(mrt_cmake_modules)

find_package(catkin REQUIRED)

catkin_package(CFG_EXTRAS mrt_cmake_modules-extras.cmake)

# Install all cmake files
install(DIRECTORY cmake DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(PROGRAMS scripts/generate_cmake_dependency_file.py scripts/init_coverage.py scripts/eval_coverage.py
                 scripts/run_test.py DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/scripts)
install(PROGRAMS scripts/generate_cmakelists.py DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
install(FILES yaml/cmake.yaml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/yaml)
install(FILES ci_templates/CMakeLists.txt DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

# Make sure the same test targets exist as in other projects. This is so that "catkin run_tests" works, etc.
if(CATKIN_ENABLE_TESTING)
    if(NOT TARGET tests)
        add_custom_target(tests)
    endif()
    if(NOT TARGET run_tests)
        # usually catkin does that for us
        add_custom_target(run_tests)
        enable_testing()
    endif()
    if(NOT TARGET run_tests_${PROJECT_NAME})
        add_custom_target(run_tests_${PROJECT_NAME})
        add_dependencies(run_tests run_tests_${PROJECT_NAME})
    endif()
endif()
