set(CMAKE_ADIUMWEBVIEW_VERSION_MAJOR    0  CACHE INT "Major qutIM-AdiumWebView version number" FORCE)
set(CMAKE_ADIUMWEBVIEW_VERSION_MINOR    1  CACHE INT "Minor qutIM-AdiumWebView version number" FORCE)
set(CMAKE_ADIUMWEBVIEW_VERSION_PATCH    0  CACHE INT "Release qutIM-AdiumWebView version number" FORCE)
set(CMAKE_ADIUMWEBVIEW_VERSION_STRING "${CMAKE_ADIUMWEBVIEW_VERSION_MAJOR}.${CMAKE_ADIUMWEBVIEW_VERSION_MINOR}.${CMAKE_ADIUMWEBVIEW_VERSION_PATCH}" CACHE STRING "qutIM-AdiumWebView version string" FORCE)

file( GLOB SRC "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp" )
file( GLOB HDR "${CMAKE_CURRENT_SOURCE_DIR}/*.h" )
file( GLOB FORMS "${CMAKE_CURRENT_SOURCE_DIR}/*.ui" )

set( ADIUMWEBVIEW_TYPE "SHARED" )
if( ADIUMWEBVIEW_BUILD_TYPE STREQUAL "STATIC" )
    set( ADIUMWEBVIEW_TYPE "STATIC" )
    message( STATUS "libqutim-adiumwebview will be linked staticaly" )
endif()
include_directories( ${CMAKE_CURRENT_BINARY_DIR} ${QT_INCLUDES} ${QT_QTWEBKIT_INCLUDE_DIR} )

qutim_wrap_cpp( MOC_SRC ${HDR} )
qt4_wrap_ui( UIS_H ${FORMS} )

add_library( qutim-adiumwebview ${ADIUMWEBVIEW_TYPE} ${SRC} ${MOC_SRC} ${UIS_H} ${HDR} )

qutim_target_link_libraries(qutim-adiumwebview
    ${QT_LIBRARIES}
	${QT_QTWEBKIT_LIBRARY}
	${QUTIM_LIBRARY}
    )

set_target_properties(qutim-adiumwebview PROPERTIES
    VERSION ${CMAKE_ADIUMWEBVIEW_VERSION_STRING}
    SOVERSION ${CMAKE_ADIUMWEBVIEW_VERSION_MAJOR}
    DEFINE_SYMBOL ADIUMWEBVIEW_LIBRARY
)

install( TARGETS qutim-adiumwebview 
	RUNTIME DESTINATION bin
		COMPONENT Core
		LIBRARY DESTINATION lib${LIB_SUFFIX}
		COMPONENT Core
		ARCHIVE DESTINATION lib${LIB_SUFFIX}
		COMPONENT CoreDevel
	BUNDLE DESTINATION library
)


install(FILES ${HDR}
	DESTINATION include/qutim/adiumwebview
	COMPONENT CoreDevel
)
qutim_generate_includes("qutim/adiumwebview" ${HDR})
