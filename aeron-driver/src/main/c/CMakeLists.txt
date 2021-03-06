# Copyright 2014-2017 Real Logic Ltd.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

SET(SOURCE
    concurrent/aeron_spsc_rb.c
    concurrent/aeron_mpsc_rb.c
    concurrent/aeron_atomic.c
    aeron_driver_context.c
    aeron_alloc.c
    aeron_driver.c)

SET(HEADERS
    concurrent/aeron_atomic.h
    concurrent/aeron_atomic64_gcc_x86_64.h
    concurrent/aeron_spsc_rb.h
    util/aeron_platform.h
    util/aeron_bitutil.h
    concurrent/aeron_rb.h
    concurrent/aeron_mpsc_rb.h
    aeronmd.h
    aeron_alloc.h)

add_library(aeron_driver SHARED ${SOURCE} ${HEADERS})
add_executable(aeronmd aeronmd.c)

set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -DDISABLE_BOUNDS_CHECKS")

target_link_libraries(
    aeron_driver
    ${CMAKE_THREAD_LIBS_INIT})

target_link_libraries(
    aeronmd
    aeron_driver
    ${CMAKE_THREAD_LIBS_INIT})

install(TARGETS aeron_driver LIBRARY DESTINATION lib)
