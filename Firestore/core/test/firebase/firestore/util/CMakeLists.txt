# Copyright 2017 Google
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

set(CMAKE_CXX_EXTENSIONS ON)

# Required to allow 0 length printf style strings for testing purposes.
if(NOT MSVC)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-format-zero-length")
endif()


## async

cc_test(
  firebase_firestore_util_async_std_test
  SOURCES
    async_queue_std_test.cc
    async_queue_test.cc
    async_queue_test.h
    async_tests_util.h
    executor_std_test.cc
    executor_test.cc
    executor_test.h
  DEPENDS
    firebase_firestore_util_async_std
)

if(HAVE_LIBDISPATCH)
  cc_test(
    firebase_firestore_util_async_libdispatch_test
    SOURCES
      async_queue_libdispatch_test.mm
      async_queue_test.cc
      async_queue_test.h
      async_tests_util.h
      executor_libdispatch_test.mm
      executor_test.cc
      executor_test.h
    DEPENDS
      firebase_firestore_util_async_libdispatch
  )
endif()


## base

if(APPLE)
  cc_test(
    firebase_firestore_util_base_apple_test
    SOURCES
      hard_assert_test.cc
      log_test.cc
      string_apple_test.mm
    DEPENDS
      firebase_firestore_util_base_apple
  )
endif(APPLE)

cc_test(
  firebase_firestore_util_base_stdio_test
  SOURCES
    hard_assert_test.cc
    log_test.cc
  DEPENDS
    firebase_firestore_util_base_stdio
)


## filesystem

if(APPLE)
  cc_test(
    firebase_firestore_util_filesystem_apple_test
    SOURCES
      filesystem_test.cc
      path_test.cc
    DEPENDS
      absl_optional
      firebase_firestore_util_autoid
      firebase_firestore_util_filesystem_apple
  )
endif()

if(NOT APPLE AND NOT WIN32)
  cc_test(
    firebase_firestore_util_filesystem_posix_test
    SOURCES
      filesystem_test.cc
      path_test.cc
    DEPENDS
      absl_optional
      firebase_firestore_util_autoid
      firebase_firestore_util_filesystem_posix
  )
endif()

if(WIN32)
  cc_test(
    firebase_firestore_util_filesystem_win_test
    SOURCES
      filesystem_test.cc
      path_test.cc
    DEPENDS
      absl_optional
      firebase_firestore_util_autoid
      firebase_firestore_util_filesystem_win
  )
endif()


## random

if(HAVE_ARC4RANDOM)
  cc_test(
    firebase_firestore_util_random_arc4random_test
    SOURCES
      secure_random_test.cc
    DEPENDS
      firebase_firestore_util_random_arc4random
  )
endif()

if(HAVE_OPENSSL_RAND_H)
  cc_test(
    firebase_firestore_util_random_openssl_test
    SOURCES
      secure_random_test.cc
    DEPENDS
      firebase_firestore_util_random_openssl
  )
endif()


## main library

cc_test(
  firebase_firestore_util_test
  SOURCES
    autoid_test.cc
    bits_test.cc
    comparison_test.cc
    delayed_constructor_test.cc
    hashing_test.cc
    iterator_adaptors_test.cc
    ordered_code_test.cc
    status_apple_test.mm
    status_test.cc
    status_testing.h
    statusor_test.cc
    strerror_test.cc
    string_format_apple_test.mm
    string_format_test.cc
    string_util_test.cc
    string_win_test.cc
    vector_of_ptr_test.cc
  DEPENDS
    absl_base
    absl_strings
    firebase_firestore_util
    GMock::GMock
)

# Helper utilities

cc_library(
  firebase_firestore_remote_test_util
  SOURCES
    create_noop_connectivity_monitor.h
    create_noop_connectivity_monitor.cc
    fake_credentials_provider.h
    fake_credentials_provider.cc
    grpc_stream_tester.h
    grpc_stream_tester.cc
  DEPENDS
    absl_base
    absl_strings
    firebase_firestore_auth
    firebase_firestore_core
    firebase_firestore_remote
    firebase_firestore_util
    grpc++
)

# Benchmarks

cc_binary(
  firebase_firestore_util_ordered_code_benchmark
  SOURCES
    ordered_code_benchmark.cc
  DEPENDS
    benchmark
    benchmark_main
    firebase_firestore_util
)

if(APPLE)
  cc_binary(
    firebase_firestore_util_string_apple_benchmark
    SOURCES
      string_apple_benchmark.mm
    DEPENDS
      benchmark
      benchmark_main
      firebase_firestore_util_base_apple
  )
endif()
