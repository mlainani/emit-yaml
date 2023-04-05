cmake_minimum_required(VERSION 3.16.3)

project(emit-yaml CXX)

include(ExternalProject)

ExternalProject_Add(
    project_yaml_cpp
    GIT_REPOSITORY https://github.com/jbeder/yaml-cpp.git
    GIT_TAG 0579ae3d976091d7d664aa9d2527e0d0cff25763 # yaml-cpp-0.7.0
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
)

ExternalProject_Get_Property(project_yaml_cpp install_dir)

add_library(libyaml STATIC IMPORTED)
set_property(TARGET libyaml PROPERTY IMPORTED_LOCATION ${install_dir}/lib/libyaml-cpp.a)
add_dependencies(libyaml project_yaml_cpp)

add_executable(${PROJECT_NAME}
               ${CMAKE_CURRENT_LIST_DIR}/main.cpp)

include_directories(${install_dir}/include)
target_link_libraries(${PROJECT_NAME} libyaml)
