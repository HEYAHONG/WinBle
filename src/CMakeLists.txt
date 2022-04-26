cmake_minimum_required(VERSION 3.8)

Set(PROJECT_NAME WinBle)

Project(${PROJECT_NAME} C CXX ASM)

set(CMAKE_CXX_STANDARD 17)

if(NOT WIN32)
message(FATAL_ERROR "只支持Windows" )
endif()

if(CMAKE_CL_64)
    set(CURRENT_PLATFORM "x64")
    message(STATUS "Current Platform is ${CURRENT_PLATFORM}")
else(CMAKE_CL_64)
    set(CURRENT_PLATFORM "x86")
    message(STATUS "Current Platform is ${CURRENT_PLATFORM}")
endif(CMAKE_CL_64)


#只支持Win10及以上系统,且必须使用UNICODE
add_definitions("-D_WIN32_WINNT=_WIN32_WINNT_WIN10" "-DUNICODE" "-DINITGUID")

#WinBle库
file(GLOB LIBWINBLE_CXX_C_FILES WinBleLib/*.cpp WinBleLib/*.c )
add_library(${PROJECT_NAME} ${LIBWINBLE_CXX_C_FILES})
target_include_directories(${PROJECT_NAME} PUBLIC WinBleLib/ )
target_link_libraries(${PROJECT_NAME} setupapi.dll rpcrt4.dll bthprops.dll devobj.dll cfgmgr32.dll bcrypt.dll )

#添加从Windows SDK中复制的lib文件(已改名),头文件
target_include_directories(${PROJECT_NAME} PUBLIC mingw/include )
target_link_libraries(${PROJECT_NAME} "-L${CMAKE_SOURCE_DIR}/mingw/lib"  "-L${CMAKE_SOURCE_DIR}/mingw/lib/x64" bluetoothapis.dll )


#WinBleTest
file(GLOB WINBLETEST_CXX_C_FILES WinBleTest/*.cpp WinBleTest/*.c )
add_executable(Test ${WINBLETEST_CXX_C_FILES})
target_link_libraries(Test ${PROJECT_NAME})

