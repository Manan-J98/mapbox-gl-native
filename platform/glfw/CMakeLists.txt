find_package(OpenGL REQUIRED)
find_package(PkgConfig REQUIRED)

pkg_search_module(GLFW glfw3 REQUIRED)

add_executable(
    mbgl-glfw
    ${PROJECT_SOURCE_DIR}/platform/glfw/main.cpp
    ${PROJECT_SOURCE_DIR}/platform/glfw/glfw_view.cpp
    ${PROJECT_SOURCE_DIR}/platform/glfw/glfw_gl_backend.cpp
    ${PROJECT_SOURCE_DIR}/platform/glfw/glfw_renderer_frontend.cpp
    ${PROJECT_SOURCE_DIR}/platform/glfw/settings_json.cpp
    ${PROJECT_SOURCE_DIR}/platform/glfw/test_writer.cpp
)

target_include_directories(
    mbgl-glfw
    PRIVATE ${GLFW_INCLUDE_DIRS}
)

include(${PROJECT_SOURCE_DIR}/vendor/cheap-ruler-cpp.cmake)

# Use target_link_directories when we move away from CMake 3.10.
target_link_libraries(
    mbgl-glfw
    PRIVATE $<$<BOOL:${GLFW_LIBRARY_DIRS}>:-L${GLFW_LIBRARY_DIRS}>
)

target_link_libraries(
    mbgl-glfw
    PRIVATE
        ${GLFW_LIBRARIES}
        Mapbox::Base::Extras::args
        Mapbox::Base::Extras::filesystem
        Mapbox::Base::Extras::rapidjson
        Mapbox::Map
        OpenGL::GL
        mbgl-compiler-options
        mbgl-vendor-cheap-ruler-cpp
)

set_property(TARGET mbgl-glfw PROPERTY FOLDER Executables)

install(TARGETS mbgl-glfw RUNTIME DESTINATION bin)
