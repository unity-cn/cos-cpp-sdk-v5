project(cossdk)

file(GLOB sdk_common_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/*.h")
file(GLOB sdk_op_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/op/*.h")
file(GLOB sdk_request_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/request/*.h")
file(GLOB sdk_response_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/response/*.h")
file(GLOB sdk_threadpool_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/threadpool/*.h")
file(GLOB sdk_trsf_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/trsf/*.h")
file(GLOB sdk_util_header "${CMAKE_CURRENT_SOURCE_DIR}/../include/util/*.h")
file(GLOB sdk_header
        ${sdk_common_header}
        ${sdk_op_header}
        ${sdk_request_header}
        ${sdk_response_header}
        ${sdk_threadpool_header}
        ${sdk_trsf_header}
        ${sdk_util_header})

file(GLOB sdk_common_src "${CMAKE_CURRENT_SOURCE_DIR}/../src/*.cpp")
file(GLOB sdk_op_src "${CMAKE_CURRENT_SOURCE_DIR}/../src/op/*.cpp")
file(GLOB sdk_request_src "${CMAKE_CURRENT_SOURCE_DIR}/../src/request/*.cpp")
file(GLOB sdk_response_src "${CMAKE_CURRENT_SOURCE_DIR}/../src/response/*.cpp")
file(GLOB sdk_trsf_src "${CMAKE_CURRENT_SOURCE_DIR}/../src/trsf/*.cpp")
file(GLOB sdk_util_src "${CMAKE_CURRENT_SOURCE_DIR}/../src/util/*.cpp")
file(GLOB sdk_src
        ${sdk_common_src}
        ${sdk_op_src}
        ${sdk_request_src}
        ${sdk_response_src}
        ${sdk_trsf_src}
        ${sdk_util_src})

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../include/)

add_library(${PROJECT_NAME} STATIC ${sdk_src} ${sdk_header})

# When use the on windows need change the boost library according to the local name
if(WIN32)
        target_link_libraries(${PROJECT_NAME} PRIVATE Poco::Foundation Poco::Net Poco::NetSSLWin Poco::Crypto Poco::Util Poco::JSON Poco::XML OpenSSL::SSL OpenSSL::Crypto ${Boost_LIBRARIES} jsoncpp_static jsoncpp_object)
else()
        target_link_libraries(${PROJECT_NAME} Poco::Foundation Poco::Net Poco::NetSSL Poco::Crypto Poco::Util Poco::JSON Poco::XML OpenSSL::SSL OpenSSL::Crypto stdc++ pthread jsoncpp_object jsoncpp_static ${Boost_LIBRARIES})
endif()

set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "cossdk")