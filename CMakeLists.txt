project(unionfs-fuse)

cmake_minimum_required(VERSION 2.0)
INCLUDE (CheckIncludeFiles)

# Set a default build type for single-configuration
# CMake generators if no build type is set.
IF(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
	SET(CMAKE_BUILD_TYPE RelWithDebInfo)
ENDIF(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)

# Select flags.
SET(CMAKE_C_FLAGS "-pipe -W -Wall -DFORTIFY_SOURCE=2")
SET(CMAKE_C_FLAGS_RELWITHDEBINFO "-O2 -g")
SET(CMAKE_C_FLAGS_RELEASE "-O2")
SET(CMAKE_C_FLAGS_DEBUG "-O0 -g -DDEBUG")

add_definitions(-D_FILE_OFFSET_BITS=64 -DFUSE_USE_VERSION=26)

option(WITH_XATTR "Enable support for extended attributes" OFF)

# .h include files
if (WITH_XATTR)
	CHECK_INCLUDE_FILES("sys/xattr.h" HAVE_XATTR)
	IF (HAVE_XATTR)
		add_definitions(-DHAVE_XATTR)
	ENDIF(HAVE_XATTR)
ENDIF (WITH_XATTR)



add_subdirectory(src)
add_subdirectory(man)
