cmake_minimum_required(VERSION 3.13)
project("libioth_picox"
    DESCRIPTION "Internet of Threads library: picoxnet module"
    HOMEPAGE_URL "https://github.com/virtualsquare/libioth_picox"
    VERSION 0.1.0
    LANGUAGES C)

include(GNUInstallDirs)
include(CheckIncludeFile)
include(CheckSymbolExists)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_FORTIFY_SOURCE=2 -O2 -pedantic -Wall -Wextra")
set(SYSTEM_IOTH_PATH ${CMAKE_INSTALL_FULL_LIBDIR}/ioth)

set(LIBS_REQUIRED ioth picoxnet)
set(HEADERS_REQUIRED ioth.h picoxnet.h)

set(CMAKE_REQUIRED_QUIET TRUE)

foreach(THISLIB IN LISTS LIBS_REQUIRED)
  find_library(LIB${THISLIB}_OK ${THISLIB})
  if(NOT LIB${THISLIB}_OK)
    message(FATAL_ERROR "library lib${THISLIB} not found")
  endif()
endforeach(THISLIB)

foreach(HEADER IN LISTS HEADERS_REQUIRED)
  check_include_file(${HEADER} ${HEADER}_OK)
  if(NOT ${HEADER}_OK)
    message(FATAL_ERROR "header file ${HEADER} not found")
  endif()
endforeach(HEADER)

add_definitions(-D_GNU_SOURCE)

add_library(ioth_picox-r SHARED ioth_picox.c)
set_target_properties(ioth_picox-r PROPERTIES PREFIX "")
target_link_libraries(ioth_picox-r picoxnet)
install(TARGETS ioth_picox-r DESTINATION ${SYSTEM_IOTH_PATH})
ADD_CUSTOM_TARGET(ioth_picox_n.so ALL DEPENDS ioth_picox-r
		COMMAND ${CMAKE_COMMAND} -E create_symlink ioth_picox-r.so ioth_picox_n.so)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/ioth_picox_n.so DESTINATION ${SYSTEM_IOTH_PATH})

# add_subdirectory(man)
add_custom_target(uninstall
  "${CMAKE_COMMAND}" -P "${PROJECT_SOURCE_DIR}/Uninstall.cmake")
