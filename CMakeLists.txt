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
