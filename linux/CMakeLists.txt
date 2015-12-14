include_directories(${JAVA_INCLUDE_PATH} ${JAVA_INCLUDE_PATH2} ${_stubDir})
add_library(jjstick${NUM_BITS} MODULE
 LinuxJoystick.cpp
 LinuxJoystick.h
 com_centralnexus_input_Joystick.cpp
 com_centralnexus_input_ExtendedJoystick.cpp
 )
 
target_link_libraries(jjstick${NUM_BITS} winmm)
set_target_properties(jjstick${NUM_BITS} PROPERTIES PREFIX "")

set_target_properties(jjstick${NUM_BITS} PROPERTIES COMPILE_FLAGS "-m${NUM_BITS}" LINK_FLAGS "-m${NUM_BITS}")

target_link_libraries(jjstick${NUM_BITS} ${JNI_LIBRARIES})

install(TARGETS jjstick${NUM_BITS} DESTINATION lib)