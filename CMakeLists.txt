cmake_minimum_required(VERSION 3.28.3)
project(Emulador-De-Arquitetura-De-Computador)

message(STATUS "CMake source directory: ${CMAKE_SOURCE_DIR}")
message(STATUS "Project source directory: ${CMAKE_SOURCE_DIR}")

if(NOT UNIX)
  message(WARNING "Unsupported operating system")
endif()

# Find  GLFW
find_package(glfw3 REQUIRED)
if (NOT glfw3_FOUND)
    message(FATAL_ERROR "GLFW not found! Please install it using:\n  sudo apt install libglfw3-dev")
endif()

find_package(OpenGL REQUIRED)
if (NOT OpenGL_FOUND)
    message(FATAL_ERROR "OpenGL not found! Please install it using:\n  sudo apt install libgl1-mesa-dev")
endif()

# Find and link OpenGL
find_package(OpenGL REQUIRED)

include_directories(
  ${PROJECT_SOURCE_DIR}/headers
  ${PROJECT_SOURCE_DIR}/third-party/imgui/headers
)

file(GLOB project_SRC "${PROJECT_SOURCE_DIR}/src/*.cpp")
# Collect .cpp files from the second folder and append them to project_SRC
file(GLOB another_folder_SRC "${PROJECT_SOURCE_DIR}/third-party/imgui/src/*.cpp")
list(APPEND project_SRC ${another_folder_SRC})



if(NOT CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
  set (flags_1 "-Wno-overloaded-virtual")
else()
  set (flags_1 "-MP /wd4251")
endif()



foreach(src_file ${project_SRC})
      set_source_files_properties(
          ${src_file}
          PROPERTIES
          COMPILE_FLAGS "${COMPILE_FLAGS} ${flags_1}"
          )
endforeach(src_file ${project_SRC})


add_executable(project
  ${project_SRC}
)

# Link GLFW and OpenGL libraries
target_link_libraries(project PRIVATE glfw ${OPENGL_LIBRARIES})
target_link_libraries(project PRIVATE OpenGL::GL)




