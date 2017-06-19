############################################################################
# This file is part of LImA, a Library for Image Acquisition
#
# Copyright (C) : 2009-2017
# European Synchrotron Radiation Facility
# BP 220, Grenoble 38043
# FRANCE
#
# This is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# This software is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, see <http://www.gnu.org/licenses/>.
############################################################################
# CMakeLists.txt for Aviex camera

SET(CMAKE_SHARED_LINKER_FLAGS "-Wl,--add-needed")

SET(AVIEX_SDK_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/sdk" CACHE PATH "location of aviex sdk")
add_definitions(-DOS_LINUX)

SET(aviex_srcs src/AviexCamera.cpp  src/AviexInterface.cpp
	 src/AviexDetInfoCtrlObj.cpp src/AviexSyncCtrlObj.cpp
	 src/AviexBinCtrlObj.cpp src/AviexRoiCtrlObj.cpp)
	 
FILE(STRINGS "VERSION" aviex_vers)
ADD_LIBRARY(limaaviex SHARED ${aviex_srcs})

TARGET_INCLUDE_DIRECTORIES(limaaviex PUBLIC "/usr/include/mx")
TARGET_INCLUDE_DIRECTORIES(limaaviex PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

FIND_LIBRARY(AVIEX_LIB_MX Mx)
IF (("${AVIEX_LIB_MX}" STREQUAL "AVIEX_LIB_MX-NOTFOUND"))
	MESSAGE(FATAL_ERROR "aviex : libmx not found, you can search it in cmake-gui or try to install it if it isn't here.")
ENDIF()

TARGET_LINK_LIBRARIES(limaaviex limacore)
TARGET_LINK_LIBRARIES(limaaviex ${AVIEX_LIB_MX})

SET_TARGET_PROPERTIES(limaaviex PROPERTIES VERSION ${aviex_vers} SOVERSION "1")

INSTALL(TARGETS limaaviex LIBRARY DESTINATION lib)


IF(COMPILE_SIP)
    SET(NAME "aviex")
    include(CameraRunSIP)
    INSTALL(FILES python/__init__.py DESTINATION "${PYTHON_SITE_PACKAGES_DIR}/Lima/Aviex")
ENDIF()
