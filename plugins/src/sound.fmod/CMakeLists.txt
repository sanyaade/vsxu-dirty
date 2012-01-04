cmake_minimum_required(VERSION 2.6)
#message("current list file is: " ${CMAKE_CURRENT_LIST_FILE})

get_filename_component(list_file_path ${CMAKE_CURRENT_LIST_FILE} PATH)
string(REGEX MATCH "[a-z.-]*$" module_id ${list_file_path})
message("*******************************************")
message("configuring vsxu module            " ${module_id})
message("*******************************************")

set(FMOD_LIB_NAME "fmodex")
if(UNIX)
  if (CMAKE_SIZEOF_VOID_P EQUAL 8)
    include_directories(${list_file_path}/fmodapi43800linux64/api/inc/)
    link_directories(${list_file_path}/fmodapi43800linux64/api/lib/)
    set(FMOD_LIB_NAME "fmodex64")
    install(
      FILES
        ${list_file_path}/fmodapi43800linux64/api/lib/libfmodex64-4.38.00.so
        ${list_file_path}/fmodapi43800linux64/api/lib/libfmodex64.so
      DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/
      COMPONENT engine)
  else()
    include_directories(${list_file_path}/fmodapi43800linux32/api/inc/)
    link_directories(${list_file_path}/fmodapi43800linux32/api/lib/)
    install(
      FILES
        ${list_file_path}/fmodapi43800linux32/api/lib/libfmodex-4.38.00.so
        ${list_file_path}/fmodapi43800linux32/api/lib/libfmodex.so
      DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/
      COMPONENT engine)
  endif()
endif()

if(WIN32)
  include_directories(${list_file_path}/fmodapi43800win/api/inc/)
  link_directories(${list_file_path}/fmodapi43800win/api/lib/)
endif()

file(GLOB LOCAL_SOURCES "*.cpp")
file(GLOB API_STUBS "api_stubs/*.cpp")

project (${module_id})
include (../cmake_globals.txt)
include(_sources)
add_library(${module_id} MODULE ${LOCAL_SOURCES} ${API_STUBS} ${SOURCES} "fftreal/fftreal.cpp")
include(../cmake_suffix.txt)

if(WIN32)
  if (CMAKE_SIZEOF_VOID_P EQUAL 8)
    install(FILES ${list_file_path}/fmodapi43800win/api/fmodex64.dll DESTINATION . COMPONENT engine)
    set(FMOD_LIB_NAME "fmodex64")
  else()
    install(FILES ${list_file_path}/fmodapi43800win/api/fmodex.dll DESTINATION . COMPONENT engine)
  endif()
endif()

target_link_libraries(${module_id} ${FMOD_LIB_NAME} vsxu_engine)

