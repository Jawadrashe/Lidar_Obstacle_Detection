cmake_minimum_required(VERSION 3.10) # FATAL_ERROR is the default and is redundant

project(playback)

# 1. SET C++ STANDARD: Use the standard CMAKE variable for C++ standard
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_EXTENSIONS OFF) # Use strict C++14

# 2. SET FLAGS: Use the standard CMAKE variable for flags
set(CMAKE_CXX_FLAGS "-Wall -Wextra") # Use standard CMAKE variable

# 3. FIND PACKAGES
find_package(PCL 1.2 REQUIRED)
find_package(Boost 1.83.0 REQUIRED COMPONENTS system filesystem) # Explicitly request 1.83.0

if (NOT PCL_FOUND)
    message(FATAL_ERROR "PCL not found. Please check installation.")
endif()
if (NOT Boost_FOUND)
    message(FATAL_ERROR "Boost not found. Please install the Boost libraries.")
endif()

# 4. DEFINE EXECUTABLE
add_executable (environment
    src/environment.cpp
    src/render/render.cpp
    src/processPointClouds.cpp
)

# 5. LINK AND INCLUDE (Modern Approach)
# Use target_link_libraries to handle all includes, definitions, and linking automatically.
target_link_libraries(environment
    # Add PCL libraries and components
    ${PCL_LIBRARIES}

    # Add Boost libraries and components
    ${Boost_LIBRARIES}
)

# Optional: Set PCL and Boost properties directly on the target (more explicit)
target_include_directories(environment PRIVATE ${PCL_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})
target_compile_definitions(environment PRIVATE ${PCL_DEFINITIONS})

# Optional: Remove specific libraries if absolutely necessary (but usually not needed with modern methods)
# list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4") # If required, do this before target_link_libraries