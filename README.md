# AviSynthPlus-master
# We need CMake 2.8.11 at least, because we use CMake features
# "Target Usage Requirements" and "Generator Toolset selection"
CMAKE_MINIMUM_REQUIRED( VERSION 2.8.11 )

project("AvsMod")

# Avoid uselessly linking to unused libraries
set(CMAKE_STANDARD_LIBRARIES "" CACHE STRING "" FORCE)
set(CMAKE_C_STANDARD_LIBRARIES "" CACHE STRING "" FORCE)
set(CMAKE_CXX_STANDARD_LIBRARIES "" CACHE STRING "" FORCE)

if(CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_CONFIGURATION_TYPES Debug Release RelWithDebInfo)
  set(CMAKE_CONFIGURATION_TYPES "${CMAKE_CONFIGURATION_TYPES}" CACHE STRING "Reset the configurations to what we need" FORCE)
endif()

IF( MSVC_IDE )  # Check for Visual Studio

  file(MAKE_DIRECTORY "${CMAKE_BINARY_DIR}/Output/plugins")
  file(MAKE_DIRECTORY "${CMAKE_BINARY_DIR}/Output/system")

  IF( NOT MSVC_VERSION VERSION_LESS 1700 )  # Check for v11 (VS 2012)
    # We want our project to also run on Windows XP
    set(CMAKE_GENERATOR_TOOLSET "v110_xp" CACHE STRING "The compiler toolset to use for Visual Studio." FORCE)
  ENDIF()

  # Enable C++ with SEH exceptions
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /EHa")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHa")

  # Prevent VC++ from complaining about not using MS-specific functions
  add_definitions("/D _CRT_SECURE_NO_WARNINGS /D _SECURE_SCL=0 /D _ITERATOR_DEBUG_LEVEL=0")
  
  # Set additional optimization flags
  set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /Oy /Ot /GS-")
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /Oy /Ot /GS-")

ENDIF()

add_subdirectory("avs_core")
add_subdirectory("plugins")
