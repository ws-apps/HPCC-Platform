################################################################################
#    HPCC SYSTEMS software Copyright (C) 2012 HPCC Systems®.
#
#    Licensed under the Apache License, Version 2.0 (the "License");
#    you may not use this file except in compliance with the License.
#    You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS,
#    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#    See the License for the specific language governing permissions and
#    limitations under the License.
################################################################################

# Component: soaplib

#####################################################
# Description:
# ------------
#    Cmake Input File for soaplib
#####################################################

project(soaplib)

set(SRCS
    ../../../platform/espcontext.cpp
    ../../../platform/espprotocol.cpp
    ../../../platform/espthread.cpp
    ../../../platform/espcache.cpp
    ../../../platform/sechandler.cpp
    ../../../platform/txsummary.cpp
    ../../../protocols/http/mapinfo.cpp
    ../../bindutil.cpp
    ../../http/client/httpclient.cpp
    ../../http/platform/htmlpage.cpp
    ../../http/platform/httpbinding.cpp
    ../../http/platform/httpprot.cpp
    ../../http/platform/httpsecurecontext.cpp
    ../../http/platform/httpservice.cpp
    ../../http/platform/httptransport.cpp
    ../../http/platform/mime.cpp
    ../../http/platform/msgbuilder.cpp
    ../client/soapclient.cpp
    ../Platform/soapbind.cpp
    ../Platform/soapmessage.cpp
    ../Platform/soapservice.cpp
    )

include_directories(
    ./../../../../system/include
    ./../../../../system/security/shared
    ./../../../../system/security/securesocket
    ./../../../../system/security/LdapSecurity
    ./../../../../system/jlib
    ./../../../bindings
    ./../../../platform
    ./../../../services/common
    ./../../../../system/xmllib
    ./..
    ./../../../bindings/SOAP/xpp
    ./../../../../system/mp 
    ./../../../../dali/base
    )

add_definitions(-DESPHTTP_EXPORTS)

if(WIN32)
    add_definitions(-D_LIB)
    HPCC_ADD_LIBRARY(soaplib STATIC ${SRCS})
else(WIN32)
    HPCC_ADD_LIBRARY(soaplib SHARED ${SRCS})
    install(TARGETS soaplib RUNTIME DESTINATION ${EXEC_DIR} LIBRARY DESTINATION ${LIB_DIR})
    target_link_libraries(soaplib dalibase jlib)
endif(WIN32)

add_dependencies(soaplib jlib espscm)

if(LIBMEMCACHED_FOUND)
    if(MEMCACHED_USE_EXTERNAL_LIBRARY)
        target_link_libraries(soaplib libmemcached libmemcachedutil)
        add_dependencies(soaplib generate-libmemcached)
        install(CODE "set(ENV{LD_LIBRARY_PATH} \"\$ENV{LD_LIBRARY_PATH}:${PROJECT_BINARY_DIR}:${CMAKE_BINARY_DIR}/build-libmemcached/libmemcached/.libs\")")
    else()
        target_link_libraries(soaplib ${LIBMEMCACHED_LIBRARIES})
    endif()
endif()
