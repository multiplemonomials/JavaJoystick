include_directories(${JAVA_INCLUDE_PATH} ${JAVA_INCLUDE_PATH2} ${_stubDir})
	
	
add_custom_command(
OUTPUT com_centralnexus_input_ExtendedJoystick.h
com_centralnexus_input_Joystick.h
COMMAND ${Java_JAVAH_EXECUTABLE} -verbose
	-classpath "${CLASSPATH_SEPARATOR}${_jarFile}"
	-d ${_stubDir}
	-jni com.centralnexus.input.ExtendedJoystick
	com.centralnexus.input.Joystick
)

if(WIN32)
	add_library(jjstick${NUM_BITS} MODULE
	 windows/Main.cpp
	 windows/resource.h
	 windows/StdAfx.h
	 windows/com_centralnexus_input_Joystick.cpp
	 windows/com_centralnexus_input_ExtendedJoystick.cpp
	 com_centralnexus_input_Joystick.h
	 com_centralnexus_input_ExtendedJoystick.h
	 windows/joystick.rc
	 )
	target_link_libraries(jjstick${NUM_BITS} winmm)
	set_target_properties(jjstick${NUM_BITS} PROPERTIES PREFIX "")
else()
	add_library(jjstick${NUM_BITS} MODULE
	 linux/LinuxJoystick.cpp
	 linux/LinuxJoystick.h
	 linux/com_centralnexus_input_Joystick.cpp
	 linux/com_centralnexus_input_ExtendedJoystick.cpp
	 com_centralnexus_input_Joystick.h
	 com_centralnexus_input_ExtendedJoystick.h
	 )
endif()

set_target_properties(jjstick${NUM_BITS} PROPERTIES COMPILE_FLAGS "-m${NUM_BITS}" LINK_FLAGS "-m${NUM_BITS}")

target_link_libraries(jjstick${NUM_BITS} ${JNI_LIBRARIES})

install(TARGETS jjstick${NUM_BITS} DESTINATION lib)
