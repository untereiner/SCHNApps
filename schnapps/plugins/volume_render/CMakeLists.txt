project(plugin_volume_render
	LANGUAGES CXX
)

find_package(cgogn_core REQUIRED)
find_package(cgogn_rendering REQUIRED)
find_package(Qt5 COMPONENTS Widgets REQUIRED)
find_package(QOGLViewer REQUIRED)

set(HEADER_FILES
	dll.h
	map_parameters.h
	volume_render.h
	volume_render_dock_tab.h
)

set(SOURCE_FILES
	volume_render.cpp
	volume_render_dock_tab.cpp
)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES})

set_target_properties(${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX "_d")
target_compile_definitions(${PROJECT_NAME} PRIVATE "-DSCHNAPPS_PLUGIN_VOLUME_RENDER_DLL_EXPORT")

target_include_directories(${PROJECT_NAME} PUBLIC
	${QOGLViewer_INCLUDE_DIRS}
	$<BUILD_INTERFACE:${SCHNAPPS_SOURCE_DIR}>
	$<BUILD_INTERFACE:${cgogn_core_INCLUDE_DIRS}>
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_BINARY_DIR}
)

if (USE_TRANSPARENCY)
	target_link_libraries(${PROJECT_NAME}
		schnapps_core
		plugin_surface_render_transp
		${cgogn_core_LIBRARIES}
		${cgogn_rendering_LIBRARIES}
		${Qt5Widgets_LIBRARIES}
		${QOGLViewer_LIBRARIES})
else()
	target_link_libraries(${PROJECT_NAME}
		schnapps_core
		${cgogn_core_LIBRARIES}
		${cgogn_rendering_LIBRARIES}
		${Qt5Widgets_LIBRARIES}
		${QOGLViewer_LIBRARIES})
endif()

set_target_properties (${PROJECT_NAME} PROPERTIES FOLDER plugins)
