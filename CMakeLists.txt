# created by liujin, for make libhdbpp-mysql on Windows
# 

project( hdb++mysql )
cmake_minimum_required( VERSION 3.10 )

if(NOT LIBHDBPP_PATH)
	message(FATAL_ERROR "Not set LIBHDBPP_PATH for libhdbpp")
endif(NOT LIBHDBPP_PATH)

if(WIN32)
	if(NOT MYSQL_PATH)
		message(FATAL_ERROR "Not set MYSQL_PATH for MySQL path on Windows Platform")
	endif(NOT MYSQL_PATH)
	if(NOT TANGO_PATH)
		message(FATAL_ERROR "Not set TANGO_PATH for Tango path on Windows Platform")
	endif(NOT TANGO_PATH)
endif(WIN32)
message("libhdbpp path:  ${LIBHDBPP_PATH}")
message("mysql path:  ${MYSQL_PATH}")
message("tango path:  ${TANGO_PATH}")

if(NOT CMAKE_BUILD_TYPE)
	message("Not set build type - default is Release")
	set(CMAKE_BUILD_TYPE Release)
endif()

# find all source file under current directory, add to SRC_LIST
aux_source_directory(./src SRC_LIST)
# aux_source_directory(./other-src OTHER_SRC_LIST)
# list(APPEND SRC_LIST ${OTHER_SRC_LIST})

add_definitions(-DLOG4TANGO_HAS_DLL -DTANGO_HAS_DLL)

include_directories("${MYSQL_PATH}/include" "${TANGO_PATH}/include" 
    "${LIBHDBPP_PATH}/include")
link_directories("${MYSQL_PATH}/lib" "${TANGO_PATH}/lib" "${LIBHDBPP_PATH}/lib")

add_library(${PROJECT_NAME} SHARED ${SRC_LIST})
if (CMAKE_BUILD_TYPE STREQUAL "Release")
	target_link_libraries(${PROJECT_NAME} omniORB4_rt.lib omniDynamic4_rt.lib 
		COS4_rt.lib omnithread_rt.lib tango.lib libzmq.lib 
		hdb++.lib libmysql.lib Ws2_32.lib)
else()
	# Todo
endif()

    
install(TARGETS ${PROJECT_NAME}
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    ARCHIVE DESTINATION lib)