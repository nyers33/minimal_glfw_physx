# references:
# http://www.opengl-tutorial.org/beginners-tutorials/tutorial-6-keyboard-and-mouse/
# PhysXSDK - Snippets - SnippetHelloWorld

set(APP_NAME minimal_glfw_physx)

set(demo_src
	imagedata.h
	imagedata.cpp

	glshader.h
	glshader.cpp
	
	glmeshdata.h
	glmeshdata.cpp
)

add_executable(${APP_NAME} main.cpp  ${demo_src})

target_link_libraries(${APP_NAME} glfw glew OpenGL::GL glm::glm)
target_link_libraries(${APP_NAME} ${Vulkan_LIBRARIES})

target_include_directories(${APP_NAME} PUBLIC ${CMAKE_CURRENT_LIST_DIR})

source_group("sources" FILES main.cpp)
source_group("sources\\util" FILES ${demo_src})

set(PHSYX_LIBS_WIN
	"PhysX_64.lib"
	"PhysXCommon_64.lib"
	"PhysXCooking_64.lib"
	"PhysXFoundation_64.lib"

	"PhysXPvdSDK_static_64.lib"
	"PhysXVehicle_static_64.lib"
	"PhysXExtensions_static_64.lib"
	"PhysXCharacterKinematic_static_64.lib"
)

set(PHSYX_LIBS_LINUX
	"libPhysX_static_64.a"
	"libPhysXCommon_static_64.a"
	"libPhysXCooking_static_64.a"
	"libPhysXFoundation_static_64.a"

	"libPhysXPvdSDK_static_64.a"
	"libPhysXVehicle_static_64.a"
	"libPhysXExtensions_static_64.a"
	"libPhysXCharacterKinematic_static_64.a"
)

# TODO: avoid hard coded platform specific physx build directory
set(PHYSX_ROOT_WIN "C:/work/PhysX")
set(PHSYX_LIBS_ROOT_DIR_WIN "C:/work/PhysX/physx/bin/win.x86_64.vc142.md")
set(PHYSX_ROOT_LINUX "/home/nyers/Work/PhysX")
set(PHSYX_LIBS_ROOT_DIR_LINUX "/home/nyers/Work/PhysX/physx/bin/linux.clang")

IF(WIN32)
	set(PHSYX_LIBS ${PHSYX_LIBS_WIN})
	set(PHSYX_LIBS_ROOT_DIR ${PHSYX_LIBS_ROOT_DIR_WIN})

	target_include_directories(${APP_NAME} PUBLIC "${PHYSX_ROOT_WIN}/physx/include" )
	target_include_directories(${APP_NAME} PUBLIC "${PHYSX_ROOT_WIN}/pxshared/include" )
ELSE(WIN32)
	set(PHSYX_LIBS ${PHSYX_LIBS_LINUX})
	set(PHSYX_LIBS_ROOT_DIR ${PHSYX_LIBS_ROOT_DIR_LINUX})

	target_include_directories(${APP_NAME} PUBLIC "${PHYSX_ROOT_LINUX}/physx/include" )
	target_include_directories(${APP_NAME} PUBLIC "${PHYSX_ROOT_LINUX}/pxshared/include" )
ENDIF(WIN32)

foreach(lib ${PHSYX_LIBS})
	target_link_libraries(${APP_NAME} debug "${PHSYX_LIBS_ROOT_DIR}/debug/${lib}")
	target_link_libraries(${APP_NAME} optimized "${PHSYX_LIBS_ROOT_DIR}/release/${lib}")
endforeach()

# windows msvc specific physx util strings
# PATH=%PATH%;C:\work\PhysX\physx\bin\win.x86_64.vc142.md\$(Configuration)
# <cmakeSwitch name="NV_USE_STATIC_WINCRT" value="False" comment="Use the statically linked windows CRT" />
