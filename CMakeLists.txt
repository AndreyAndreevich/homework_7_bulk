cmake_minimum_required(VERSION 3.2)

if($ENV{TRAVIS_BUILD_NUMBER})
        project(bulk VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
        project(bulk VERSION 0.0.1)  
endif()

find_package(Boost COMPONENTS unit_test_framework REQUIRED)

set(SOURCE 
        Handler.cpp
        Writers.cpp    
        Parser.cpp
)

add_executable(${PROJECT_NAME} ${SOURCE} main.cpp)

set(TEST_NAME bulk_test)

add_executable(${TEST_NAME} ${SOURCE} bulk_test.cpp)

set_target_properties(${PROJECT_NAME} ${TEST_NAME} PROPERTIES
        CXX_STANDARD 14
        CXX_STANDARD_REQUIRED ON
        COMPILE_OPTIONS -Wpedantic -Wall -Wextra
        )

set_target_properties(${TEST_NAME} PROPERTIES
        COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK 
        INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR} 
        )

target_link_libraries(${TEST_NAME}
        ${Boost_LIBRARIES}
        )

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR ${PROJECT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PROJECT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})

set(CPACK_PACKAGE_CONTACT l.a.r.p@yandex.ru)

include(CPack)

enable_testing()

add_test(${TEST_NAME} ${TEST_NAME})