# Copyright (c) Huawei Technologies Co., Ltd. 2020. All rights reserved.
# secGear is licensed under the Mulan PSL v2.
# You can use this software according to the terms and conditions of the Mulan PSL v2.
# You may obtain a copy of Mulan PSL v2 at:
#     http://license.coscl.org.cn/MulanPSL2
# THIS SOFTWARE IS PROVIDED ON AN "AS IS" BASIS, WITHOUT WARRANTIES OF ANY KIND, EITHER EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO NON-INFRINGEMENT, MERCHANTABILITY OR FIT FOR A PARTICULAR
# PURPOSE.
# See the Mulan PSL v2 for more details.

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fvisibility=hidden -fPIC")

set(LIBRARY_INSTALL /lib64)

include_directories(${LOCAL_ROOT_PATH}/inc/host_inc)

if(CC_GP)
    add_subdirectory(gp)
endif()

if(CC_SGX)
    add_subdirectory(sgx)
endif()

add_library(secgear SHARED enclave.c enclave_internal.c)

target_link_libraries(secgear dl)

set_target_properties(secgear PROPERTIES SKIP_BUILD_RPATH TRUE)

install(TARGETS  secgear
        LIBRARY
        DESTINATION ${LIBRARY_INSTALL}
        PERMISSIONS  OWNER_EXECUTE OWNER_WRITE OWNER_READ GROUP_READ  GROUP_EXECUTE WORLD_READ  WORLD_EXECUTE)