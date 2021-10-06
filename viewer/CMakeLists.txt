
add_executable(viewer main.cpp)
target_link_libraries(viewer PRIVATE
        Corrade::Main
        Magnum::GL
        Magnum::Magnum
        Magnum::MeshTools
        Magnum::SceneGraph
        Magnum::Shaders
        Magnum::Trade)

if(CORRADE_TARGET_ANDROID)
    find_package(Magnum REQUIRED AndroidApplication)
    target_link_libraries(viewer PRIVATE Magnum::AndroidApplication)
elseif(CORRADE_TARGET_EMSCRIPTEN)
    find_package(Magnum REQUIRED Sdl2Application)
    target_link_libraries(viewer PRIVATE Magnum::Sdl2Application)
    target_link_options(viewer PRIVATE "-flto -O3")

    add_custom_command(TARGET viewer POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${MAGNUM_EMSCRIPTENAPPLICATION_JS}
            ${MAGNUM_WEBAPPLICATION_CSS}
            $<TARGET_FILE_DIR:viewer>
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${CMAKE_CURRENT_SOURCE_DIR}/application.html
            $<TARGET_FILE_DIR:viewer>/viewer.html)

    emscripten_embed_file(viewer ${CONAN_STL} ${CONAN_STL})
    find_package(MagnumPlugins REQUIRED StlImporter)
    target_link_libraries(viewer PRIVATE MagnumPlugins::StlImporter)
else()
    find_package(Magnum REQUIRED Sdl2Application)
    target_link_libraries(viewer PRIVATE Magnum::Sdl2Application)
endif()
