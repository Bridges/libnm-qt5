cmake_minimum_required(VERSION 2.8.8)

option(USE_QT5 "Using Qt5" OFF)
set(VERSION 0.9.0)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_MODULE_PATH})

set(LIB_SUFFIX "" CACHE STRING "Define suffix of library directory name (32/64)" )

find_package(PkgConfig)
if(USE_QT5)
    set(libraryName NetworkManagerQt5)
    project(libNetworkManagerQt5)
    find_package(Qt5Core REQUIRED)
    find_package(Qt5DBus REQUIRED)

    set(CMAKE_AUTOMOC ON)
    add_definitions(-std=c++11 -DUSE_QT5)
    pkg_check_modules(LIBMODEMMANAGERQT REQUIRED ModemManagerQt5)
    if(NOT LIBMODEMMANAGERQT_FOUND)
        message(FATAL_ERROR "ERROR: libModemManagerQt5 not found")
    endif(NOT LIBMODEMMANAGERQT_FOUND)
else()
    add_definitions(${QT_DEFINITIONS})
    set(libraryName NetworkManagerQt)
    project(libNetworkManagerQt)
    find_package(Qt4 REQUIRED QtCore QtNetwork)
    pkg_check_modules(LIBMODEMMANAGERQT REQUIRED ModemManagerQt)
    if(NOT LIBMODEMMANAGERQT_FOUND)
        message(FATAL_ERROR "ERROR: libModemManagerQt not found")
    endif(NOT LIBMODEMMANAGERQT_FOUND)
endif()

find_package(NetworkManager 0.9.8.0 REQUIRED)

include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR} ${QT_INCLUDES}
                    ${LIBMODEMMANAGERQT_INCLUDE_DIRS} ${CMAKE_INCLUDE_PATH})

link_directories(${LIBMODEMMANAGERQT_LIBRARY_DIRS} ${CMAKE_LIBRARY_PATH})

include_directories(
   ${NETWORKMANAGER_INCLUDE_DIRS}
   ${NM-UTIL_INCLUDE_DIRS}
   ${NM-GLIB_INCLUDE_DIRS}
   ${CMAKE_CURRENT_SOURCE_DIR}
   ${CMAKE_CURRENT_SOURCE_DIR}/dbus
   ${CMAKE_CURRENT_SOURCE_DIR}/settings
   ${CMAKE_CURRENT_BUILD_DIR}
)

#add_subdirectory(dbus)

option(BUILD_EXAMPLES "Build example applications" FALSE)
if (BUILD_EXAMPLES)
    add_subdirectory(examples)
endif(BUILD_EXAMPLES)

enable_testing(true)

add_subdirectory(tests)

set(NetworkManagerQt_HEADERS
    accesspoint.h
    activeconnection.h
    adsldevice.h
    bluetoothdevice.h
    bonddevice.h
    bridgedevice.h
    connection.h
    device.h
    dhcp4config.h
    dhcp6config.h
    infinibanddevice.h
    ipaddress.h
    iproute.h
    ipconfig.h
    manager.h
    modemdevice.h
    olpcmeshdevice.h
    secretagent.h
    settings.h
    utils.h
    vlandevice.h
    vpnconnection.h
    vpnplugin.h
    wimaxdevice.h
    wimaxnsp.h
    wireddevice.h
    wirelessdevice.h
    wirelessnetwork.h
    generic-types.h
    NetworkManagerQt-export.h
)

set(NetworkManagerQt_PRIVATE_HEADERS
    manager_p.h
    settings_p.h
    #settings/template_p.h
)

set(NetworkManagerQt_PART_SRCS
    accesspoint.cpp
    activeconnection.cpp
    adsldevice.cpp
    bluetoothdevice.cpp
    bonddevice.cpp
    bridgedevice.cpp
    connection.cpp
    device.cpp
    dhcp4config.cpp
    dhcp6config.cpp
    infinibanddevice.cpp
    ipaddress.cpp
    iproute.cpp
    ipconfig.cpp
    #nmpropertyhelper.cpp
    manager.cpp
    modemdevice.cpp
    olpcmeshdevice.cpp
    secretagent.cpp
    settings.cpp
    utils.cpp
    vlandevice.cpp
    vpnconnection.cpp
    vpnplugin.cpp
    wimaxdevice.cpp
    wimaxnsp.cpp
    wireddevice.cpp
    wirelessdevice.cpp
    wirelessnetwork.cpp
    generic-types.cpp
)

set(DBUS_INTERFACE_SRCS
   dbus/nm-access-pointinterface.cpp
   dbus/nm-active-connectioninterface.cpp
   dbus/nm-agent-managerinterface.cpp
   dbus/nm-device-adslinterface.cpp
   dbus/nm-device-bondinterface.cpp
   dbus/nm-device-bridgeinterface.cpp
   dbus/nm-device-btinterface.cpp
   dbus/nm-device-ethernetinterface.cpp
   dbus/nm-device-infinibandinterface.cpp
   dbus/nm-device-modeminterface.cpp
   dbus/nm-device-olpc-meshinterface.cpp
   dbus/nm-device-vlaninterface.cpp
   dbus/nm-device-wifiinterface.cpp
   dbus/nm-device-wimaxinterface.cpp
   dbus/nm-deviceinterface.cpp
   dbus/nm-dhcp4-configinterface.cpp
   dbus/nm-dhcp6-configinterface.cpp
   dbus/nm-ip4-configinterface.cpp
   dbus/nm-ip6-configinterface.cpp
   dbus/nm-managerinterface.cpp
   dbus/nm-ppp-managerinterface.cpp
   #dbus/nm-secret-agentinterface.cpp
   dbus/nm-secret-agentadaptor.cpp
   dbus/nm-settings-connectioninterface.cpp
   dbus/nm-settingsinterface.cpp
   dbus/nm-vpn-connectioninterface.cpp
   dbus/nm-vpn-plugininterface.cpp
   dbus/nm-wimax-nspinterface.cpp
   )

set(CONNECTIONS_SETTINGS_SRCS
    settings/wiredsetting.cpp
    settings/olpcmeshsetting.cpp
    settings/wirelesssetting.cpp
    settings/wirelesssecuritysetting.cpp
    settings/security8021xsetting.cpp
    settings/adslsetting.cpp
    settings/bluetoothsetting.cpp
    settings/bondsetting.cpp
    settings/bridgesetting.cpp
    settings/bridgeportsetting.cpp
    settings/cdmasetting.cpp
    settings/connectionsettings.cpp
    settings/gsmsetting.cpp
    settings/ipv4setting.cpp
    settings/ipv6setting.cpp
    settings/infinibandsetting.cpp
    settings/pppsetting.cpp
    settings/pppoesetting.cpp
    settings/setting.cpp
    settings/serialsetting.cpp
    #settings/template.cpp
    settings/vlansetting.cpp
    settings/vpnsetting.cpp
    settings/wimaxsetting.cpp
    )

set(CONNECTIONS_SETTINGS_HEADERS
    settings/wiredsetting.h
    settings/olpcmeshsetting.h
    settings/wirelesssetting.h
    settings/wirelesssecuritysetting.h
    settings/security8021xsetting.h
    settings/adslsetting.h
    settings/bluetoothsetting.h
    settings/bondsetting.h
    settings/bridgesetting.h
    settings/bridgeportsetting.h
    settings/cdmasetting.h
    settings/connectionsettings.h
    settings/gsmsetting.h
    settings/ipv4setting.h
    settings/ipv6setting.h
    settings/infinibandsetting.h
    settings/pppsetting.h
    settings/pppoesetting.h
    settings/setting.h
    settings/serialsetting.h
    #settings/template.h
    settings/vlansetting.h
    settings/vpnsetting.h
    settings/wimaxsetting.h
    )

if(NOT USE_QT5)
    message(STATUS "Wrapping moc")
    qt4_wrap_cpp(NetworkManagerQt_MOC ${NetworkManagerQt_PRIVATE_HEADERS})
    qt4_automoc(${NetworkManagerQt_PART_SRCS})
    qt4_automoc(${DBUS_INTERFACE_SRCS})
endif()

add_library(${libraryName} SHARED ${NetworkManagerQt_PART_SRCS} ${DBUS_INTERFACE_SRCS} ${CONNECTIONS_SETTINGS_SRCS} ${NetworkManagerQt_MOC} )
if(USE_QT5)
    qt5_use_modules(${libraryName} Core DBus Network)
endif()

# TODO: dlopen libModemManagerQt to make it a runtime dependency instead of compile
# time dependency.
if(USE_QT5)
    target_link_libraries(${libraryName} ModemManagerQt5)
else()
    target_link_libraries(${libraryName} ModemManagerQt ${QT_QTCORE_LIBRARY} ${QT_QTDBUS_LIBRARY} ${QT_QTNETWORK_LIBRARY})
endif()

install(TARGETS ${libraryName} DESTINATION lib${LIB_SUFFIX})

install( FILES
   ${NetworkManagerQt_HEADERS}
   DESTINATION
   include/${libraryName} COMPONENT Devel)

install( FILES
   ${CONNECTIONS_SETTINGS_HEADERS}
   DESTINATION
   include/${libraryName}/settings COMPONENT Devel)

set_property(TARGET ${libraryName} PROPERTY COMPILE_DEFINITIONS MAKE_NMQT_LIB)
set_property(TARGET ${libraryName} PROPERTY VERSION ${VERSION})
set_property(TARGET ${libraryName} PROPERTY SOVERSION 0)

configure_file(${CMAKE_SOURCE_DIR}/${libraryName}.pc.cmake ${CMAKE_BINARY_DIR}/${libraryName}.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/${libraryName}.pc DESTINATION "${CMAKE_INSTALL_PREFIX}/lib${LIB_SUFFIX}/pkgconfig/")
message(STATUS "Writing pkg-config file...")
configure_file(${CMAKE_SOURCE_DIR}/${libraryName}.pc.cmake ${CMAKE_BINARY_DIR}/${libraryName}.pc @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/${libraryName}.pc DESTINATION "${CMAKE_INSTALL_PREFIX}/lib${LIB_SUFFIX}/pkgconfig/")

add_subdirectory(include)
