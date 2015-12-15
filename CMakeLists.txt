cmake_minimum_required(VERSION 2.8)

#hush, annoying warnings
if(POLICY CMP0017)
 cmake_policy(SET CMP0017 NEW)
endif(POLICY CMP0017)

if(POLICY CMP0045)
 cmake_policy(SET CMP0045 NEW)
endif(POLICY CMP0045)

set(CMAKE_LEGACY_CYGWIN_WIN32 0)

project(JavaJoystick)

find_package(Java REQUIRED)
find_package(JNI REQUIRED)
include(UseJava)

enable_testing()

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
	set(HOST_IS_64BIT true)
else()
	set(HOST_IS_64BIT false)
endif()

message("Host is 64 bit: ${HOST_IS_64BIT}")
	
set(64BIT_NATIVE_LIBRARY ${HOST_IS_64BIT} CACHE BOOL "Whether to build the 64 bit jni library instead of the 32 bit one.")

if(64BIT_NATIVE_LIBRARY)
	set(NUM_BITS 64)
else()
	set(NUM_BITS 32)
endif()

#Deal with Java's weird classpath prefix thing
if(WIN32)
	set(CLASSPATH_SEPARATOR ".;")
else()
	set(CLASSPATH_SEPARATOR ".:")
endif()

#for now, just install to the source directory
set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR})

#compile java to class files
set(CMAKE_JAVA_COMPILE_FLAGS "-source" "1.7" "-target" "1.7")
add_subdirectory(com/centralnexus)

set (_stubDir "${CMAKE_CURRENT_BINARY_DIR}")
get_target_property(_jarFile JavaJoystick JAR_FILE)
get_target_property(_classDir JavaJoystick CLASSDIR)


# generate library
add_subdirectory(native)

# add test to run the test window

add_test(NAME TestJoystickWindow
    COMMAND ${Java_JAVA_EXECUTABLE} 
	-Djava.library.path=${CMAKE_BINARY_DIR}/native
	-classpath "${CLASSPATH_SEPARATOR}${_jarFile}" com.centralnexus.test.WindowTest -d:0.25 -d2:0.1 -i:60)
