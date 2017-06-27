###########################################################################
# This file is part of LImA, a Library for Image Acquisition
#
#  Copyright (C) : 2009-2017
#  European Synchrotron Radiation Facility
#  CS40220 38043 Grenoble Cedex 9 
#  FRANCE
# 
#  Contact: lima@esrf.fr
# 
#  This is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation; either version 3 of the License, or
#  (at your option) any later version.
# 
#  This software is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
# 
#  You should have received a copy of the GNU General Public License
#  along with this program; if not, see <http://www.gnu.org/licenses/>.
############################################################################

set(CMAKE_SHARED_LINKER_FLAGS "-Wl,--add-needed")

set(AVIEX_SDK_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/sdk" CACHE PATH "location of aviex sdk")
add_definitions(-DOS_LINUX)

set(aviex_srcs src/AviexCamera.cpp  src/AviexInterface.cpp
	 src/AviexDetInfoCtrlObj.cpp src/AviexSyncCtrlObj.cpp
	 src/AviexBinCtrlObj.cpp src/AviexRoiCtrlObj.cpp)
	 
file(STRINGS "VERSION" aviex_vers)
add_library(limaaviex SHARED ${aviex_srcs})

target_include_directories(limaaviex PUBLIC "/usr/include/mx")
target_include_directories(limaaviex PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

find_library(AVIEX_LIB_MX Mx)
if (("${AVIEX_LIB_MX}" STREQUAL "AVIEX_LIB_MX-NOTFOUND"))
	message(FATAL_ERROR "aviex : libmx not found, you can search it in cmake-gui or try to install it if it isn't here.")
endif()

target_link_libraries(limaaviex limacore)
target_link_libraries(limaaviex ${AVIEX_LIB_MX})

set_target_properties(limaaviex PROPERTIES VERSION ${aviex_vers} SOVERSION "1")

install(TARGETS limaaviex LIBRARY DESTINATION lib)


if(COMPILE_SIP)
    set(NAME "aviex")
    include(CameraRunSIP)
    install(FILES python/__init__.py DESTINATION "${PYTHON_SITE_PACKAGES_DIR}/Lima/Aviex")
endif()
