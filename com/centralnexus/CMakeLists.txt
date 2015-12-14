add_jar(JavaJoystick input/ExtendedJoystick.java input/Joystick.java input/JoystickListener.java input/JoystickNotifier.java test/WindowTest.java)
get_target_property(_jarFile JavaJoystick JAR_FILE)
get_target_property(_classDir JavaJoystick CLASSDIR)

install_jar(JavaJoystick DESTINATION ${CMAKE_INSTALL_PREFIX})