cmake_minimum_required(VERSION 3.7)

if(${CMAKE_VERSION} VERSION_LESS 3.21)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
else()
    cmake_policy(VERSION 3.21)
endif()

# C/C++ FLAGS
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

project(HW0 VERSION 0.0.1)

if (NOT WIN32)
set(CMAKE_MODULE_PATH /usr/local/lib/cmake /usr/local/lib/x86_64-linux-gnu/cmake)
set(CMAKE_PREFIX_PATH /usr/local/lib/cmake/glfw)
endif (NOT WIN32)

find_package (PkgConfig REQUIRED)
find_package (OpenGL REQUIRED)
find_package (GLUT REQUIRED)
include_directories( ${OPENGL_INCLUDE_DIRS}  ${GLUT_INCLUDE_DIRS} )

if (WIN32)
  # Download and link to the correct folders (I have no WIN)
  # include_directories("C:/GLFW/include" "C:/glm" "C:/glew/include")
  # link_directories("C:/glew/lib/Release/x64" "C:/GLFW/lib-mingw" )
else (WIN32)
  # find_package (GLM REQUIRED)
endif (WIN32)
if (APPLE)
  include_directories(/usr/local/include)
  find_library(COCOA_LIBRARY Cocoa REQUIRED)
  find_library(IOKIT_LIBRARY IOKit REQUIRED)
  find_library(COREVID_LIBRARY CoreVideo REQUIRED)
endif (APPLE)

add_subdirectory(third_party/FreeImage-Cmake)

# Gather list of header and source files for compilation
file(GLOB_RECURSE CPP_FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/src/**/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/include/**/*.cpp
)
file(GLOB_RECURSE HPP_FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.hpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/**/*.hpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/include/**/*.hpp
)

# Gather list of header and source files for compilation
file(GLOB_RECURSE SOURCE_FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.hpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/shaders/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/shaders/*.hpp
)

## Assets
# form https://www.reddit.com/r/cmake/comments/ms89nx/building_a_game_with_cmake_how_do_i_move_my_asset/
set(link_src "${CMAKE_SOURCE_DIR}/Data")
if(UNIX AND NOT APPLE) 
    set(link_dst "${CMAKE_CURRENT_BINARY_DIR}/Data") 
else() 
    # set(link_dst "${CMAKE_CURRENT_BINARY_DIR}/$<CONFIG>/Data") 
    set(link_dst "${CMAKE_CURRENT_BINARY_DIR}/Data") 
endif() 

# Create Symlink to your Data
execute_process(COMMAND ${CMAKE_COMMAND} -E create_symlink ${link_src} ${link_dst})

## "sample" is the name of the executable
add_executable(${PROJECT_NAME} ${SOURCE_FILES})

# glm
add_subdirectory(third_party/glm EXCLUDE_FROM_ALL)
target_link_libraries (${PROJECT_NAME}
        ${GLFW3_LIBRARY}
        ${OPENGL_LIBRARIES}
        ${GLEW_LIBRARY}
        ${COCOA_LIBRARY} ${COREVID_LIBRARY} ${IOKIT_LIBRARY}
        FreeImage
        glm
)

if (WIN32)
  target_link_libraries (${PROJECT_NAME}
          ${OPENGL_LIBRARIES} glfw3 glew32s glu32 opengl32)
endif (WIN32)

if (UNIX)
  target_link_libraries (${PROJECT_NAME}
          ${OPENGL_LIBRARIES}
          ${GLFW3_LIBRARY}
          ${GLEW_LIBRARY}
          ${GLUT_LIBRARY}
          FreeImage
          glm
  )
endif (UNIX)