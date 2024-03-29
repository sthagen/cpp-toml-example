cmake_minimum_required(VERSION 3.7...3.23)
if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

# ---- Project ----

project(toml_example LANGUAGES CXX)

# ---- Fetch CPM ----

set(CPM_DOWNLOAD_VERSION 0.27.2) 
set(CPM_DOWNLOAD_LOCATION "${CMAKE_BINARY_DIR}/cmake/CPM_${CPM_DOWNLOAD_VERSION}.cmake")

if(NOT (EXISTS ${CPM_DOWNLOAD_LOCATION}))
  message(STATUS "Downloading CPM.cmake v${CPM_DOWNLOAD_VERSION}")
  file(DOWNLOAD https://github.com/TheLartians/CPM.cmake/releases/download/v${CPM_DOWNLOAD_VERSION}/CPM.cmake ${CPM_DOWNLOAD_LOCATION})
endif()

include(${CPM_DOWNLOAD_LOCATION})

# ---- Add dependencies via CPM ----
# See https://github.com/TheLartians/CPM.cmake for details and examples

CPMAddPackage(
  NAME tomlplusplus
  GIT_REPOSITORY https://github.com/sthagen/tomlplusplus.git
  VERSION 2.3.0                                  
  # 2.3.0 is released by Mark Gillard (upstream), but we read from a fork so use the associated commit directly
  GIT_TAG 4face4d5bf16326aca0da1fb33876dbca63b6e2f 
)

if(tomlplusplus_ADDED)
  add_library(tomlplusplus INTERFACE)
  target_include_directories(tomlplusplus INTERFACE ${tomlplusplus_SOURCE_DIR}/include/)
  message("Added ${tomlplusplus_SOURCE_DIR}/include/ as include")
endif()

# ---- Create executable ----

add_executable(toml_example main.cpp)
set_target_properties(toml_example PROPERTIES CXX_STANDARD 17)

target_link_libraries(toml_example tomlplusplus)
