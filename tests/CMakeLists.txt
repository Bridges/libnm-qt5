set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR})

if(USE_QT5)
    find_package(Qt5Test REQUIRED)
else()
    find_package(Qt4 REQUIRED QtTest)
endif()
include_directories(${CMAKE_CURRENT_BINARY_DIR}
                     ${CMAKE_CURRENT_SOURCE_DIR}
)

MACRO(LIBNM_TESTS)
       FOREACH(_testname ${ARGN})
            SET(test_${_testname}_SRCS ${_testname}.cpp)
            if(NOT USE_QT5)
                qt4_automoc(${test_${_testname}_SRCS})
            endif()
            add_executable(test_${_testname} ${test_${_testname}_SRCS} ${test_${_testname}_HDRS} ${test_additional})
            if(USE_QT5)
                target_link_libraries(test_${_testname}
                    NetworkManagerQt5)
                qt5_use_modules(test_${_testname} Core DBus Network Test)
            else()
                target_link_libraries(test_${_testname}
                    NetworkManagerQt ${QT_QTTEST_LIBRARY})
            endif()
            ADD_TEST(test_${_testname} test_${_testname})
       ENDFOREACH(_testname)
ENDMACRO(LIBNM_TESTS)

LIBNM_TESTS(
    8021xsetting
    adslsetting
    bluetoothsetting
    bondsetting
    bridgesetting
    bridgeportsetting
    cdmasetting
    connectionsetting
    gsmsetting
    infinibandsetting
    ipv4setting
    ipv6setting
    olpcmeshsetting
    pppsetting
    pppoesetting
    serialsetting
    vlansetting
    wimaxsetting
    wiredsetting
    wirelesssetting
    wirelesssecuritysetting
)
