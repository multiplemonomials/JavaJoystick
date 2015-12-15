add_jar(JavaJoystick
 input/ExtendedJoystick.java
 input/Joystick.java 
 input/JoystickListener.java 
 input/JoystickNotifier.java 
 test/WindowTest.java)

install_jar(JavaJoystick ${CMAKE_INSTALL_PREFIX})
