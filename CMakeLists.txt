# The MIT License (MIT)
#
# Copyright (c) 2020 Scott Aron Bloom
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sub-license, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

cmake_minimum_required(VERSION 3.21)
if(CMAKE_VERSION VERSION_LESS "3.7.0")
    set(CMAKE_INCLUDE_CURRENT_DIR ON)
endif()

project(root-TimeStamp ) 

SET(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/SABUtils/Modules ${CMAKE_MODULE_PATH})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED true)
find_package(Threads)
find_package(Qt5 COMPONENTS Core Widgets Multimedia REQUIRED)
find_package(Deploy REQUIRED)
find_package(AddUnitTest REQUIRED)
option( gtest_force_shared_crt "Use shared ( DLL ) run-time lib even when Google Test is built as static lib." ON ) 
add_subdirectory( ${CMAKE_SOURCE_DIR}/googletest ) 
set_target_properties( gmock PROPERTIES FOLDER 3rdParty/google  )
set_target_properties( gmock_main PROPERTIES FOLDER 3rdParty/google )
set_target_properties( gtest PROPERTIES FOLDER 3rdParty/google )
set_target_properties( gtest_main PROPERTIES FOLDER 3rdParty/google )
set_property( GLOBAL PROPERTY USE_FOLDERS ON )

file( REAL_PATH ~/bin/FileRenamer CMAKE_INSTALL_PREFIX EXPAND_TILDE)
#SET( CMAKE_INSTALL_PREFIX C:/Users/sbloom/bin/FileRenamer )

INSTALL( CODE "
        MESSAGE( STATUS \"Making a \${CMAKE_INSTALL_CONFIG_NAME} install area\" )
        IF( \${CMAKE_INSTALL_CONFIG_NAME} MATCHES \"^([Dd][Ee][Bb][Uu][Gg])$\" )
            SET( CMAKE_INSTALL_PREFIX \${CMAKE_INSTALL_PREFIX}.debug )
        ENDIF(  )
        MESSAGE( STATUS \"Installing to \${CMAKE_INSTALL_PREFIX}\" )
    " )

add_subdirectory( SABUtils )
add_subdirectory( MainWindow )
add_subdirectory( main )

SET( CPACK_PACKAGE_VENDOR "Scott Aron Bloom" )
SET( CPACK_PACKAGE_VERSION_MAJOR "1" )
SET( CPACK_PACKAGE_VERSION_MINOR "0" )
SET( CPACK_PACKAGE_VERSION_PATCH "0" )
SET( CPACK_GENERATOR ZIP NSIS)
SET( CPACK_NSIS_MUI_ICON ${CMAKE_SOURCE_DIR}/MainWindow/resources/application.ico )
SET( CPACK_NSIS_MUI_UNIICON ${CMAKE_SOURCE_DIR}/MainWindow/resources/application.ico )
include( CPack )
