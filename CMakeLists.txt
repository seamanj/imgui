cmake_minimum_required(VERSION 3.3)
project(imgui)



set(root ${CMAKE_CURRENT_SOURCE_DIR})

set(hdrs ${root}/imgui.h ${root}/imgui_internal.h ${root}/imstb_rectpack.h ${root}/imstb_textedit.h ${root}/imstb_truetype.h ${root}/imconfig.h)
set(srcs ${root}/imgui.cpp  ${root}/imgui_draw.cpp  ${root}/imgui_demo.cpp  ${root}/imgui_tables.cpp  ${root}/imgui_widgets.cpp)

file(GLOB backends_files "${root}/backends/*.*")

add_library(imgui ${srcs})



# tj : INSTALL_INTERFACE - Specifies the include directory to be used when the target is installed
target_include_directories(imgui INTERFACE
    $<BUILD_INTERFACE:${root}>
    $<INSTALL_INTERFACE:include>)

target_compile_definitions(imgui INTERFACE IMGUI_DISABLE_INCLUDE_IMCONFIG_H)


install(TARGETS imgui EXPORT imgui-targets DESTINATION lib)
install(EXPORT imgui-targets DESTINATION lib/cmake/imgui)



install(FILES ${hdrs} DESTINATION include)
install(FILES ${backends_files} DESTINATION include/backends)



# create config-module
set(cm ${CMAKE_CURRENT_BINARY_DIR}/hide/imgui-config.cmake)
file(WRITE ${cm} "include(\${CMAKE_CURRENT_LIST_DIR}/imgui-targets.cmake)\n")
install(FILES ${cm} DESTINATION lib/cmake/imgui)
