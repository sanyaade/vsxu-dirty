cmake_minimum_required(VERSION 2.6)
#message("current list file is: " ${CMAKE_CURRENT_LIST_FILE})

get_filename_component(list_file_path ${CMAKE_CURRENT_LIST_FILE} PATH)
string(REGEX MATCH "[a-z.-]*$" module_id ${list_file_path})
message("*******************************************")
message("configuring vsxu module            " ${module_id})
message("*******************************************")

if(UNIX)
  include_directories(lib/linux32/api/inc/)
  link_directories(lib/linux32/api/lib/)
endif(UNIX)

if(WIN32)
  include_directories(lib/win32/api/inc/)
  link_directories(lib/win32/api/lib/)
endif(WIN32)

file(GLOB LOCAL_SOURCES "*.cpp")
file(GLOB API_STUBS "api_stubs/*.cpp")

project (${module_id})
include (../cmake_globals.txt)
include(_sources)
add_library(${module_id} MODULE ${LOCAL_SOURCES} ${API_STUBS} ${SOURCES} "fftreal/fftreal.cpp")
include(../cmake_suffix.txt)
target_link_libraries(${module_id} fmodex vsxu_engine)

if(UNIX)
  install(
    FILES
      lib/linux32/api/lib/libfmodex-4.30.03.so
      lib/linux32/api/lib/libfmodex.so
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/)
endif(UNIX)

if(WIN32)
  install(
    FILES
      lib/win32/api/lib/libfmodex-4.30.03.so
      lib/linux32/api/lib/libfmodex.so
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/)
endif(WIN32)