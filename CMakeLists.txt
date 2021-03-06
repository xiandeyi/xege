cmake_minimum_required (VERSION 3.14)

project (XEGE)

math(EXPR TARGET_BITS "8 * ${CMAKE_SIZEOF_VOID_P}")

if (MSVC)
  if (TARGET_BITS EQUAL 64)
    message(STATUS "MSVC 64bit target")
    set(LIB_NAME graphics64)
  elseif (TARGET_BITS EQUAL 32)
    message(STATUS "MSVC 32bit target")
    set(LIB_NAME graphics)
  else ()
    # 非 32/64 位的 MSVC 版本
    message(WARNING "Unknown ${TARGET_BITS}bit MSVC Version: ${MSVC_VERSION}")
    set(LIB_NAME graphics)
  endif ()
elseif (CMAKE_CXX_COMPILER_ID MATCHES "GNU")
  if (TARGET_BITS EQUAL 64)
    message(STATUS "64bit GCC target")
    # 编译结果：libgraphics64.a
    set(LIB_NAME graphics64)
  elseif (TARGET_BITS EQUAL 32)
    message(STATUS "32bit GCC target")
    # 编译结果：libgraphics.a
    set(LIB_NAME graphics)
  else ()
    message(WARNING "Unknown ${TARGET_BITS}bit GCC target")
    set(LIB_NAME graphics)
  endif ()
else ()
  if (CMAKE_CXX_COMPILER_ID)
    message(WARNING "Unknown Compiler: ${CMAKE_CXX_COMPILER_ID}")
  else ()
    message(WARNING "Unknown Compiler: ${CMAKE_CXX_COMPILER}")
  endif ()
  set(LIB_NAME "graphics${TARGET_BITS}")
endif ()

add_library(${LIB_NAME} STATIC)

add_executable(graphicstest)

add_subdirectory(src)

if (EXISTS ${PROJECT_SOURCE_DIR}/temp/CMakeLists.txt)
  add_subdirectory(temp)
endif ()

option(BUILD_DEMOS "Build demos" OFF)

if (BUILD_DEMOS)
  add_subdirectory(demo)
endif ()
