cmake_minimum_required(VERSION 2.8)

# VitaSDK defines
if( NOT DEFINED CMAKE_TOOLCHAIN_FILE )
  if( DEFINED ENV{VITASDK} )
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

# Project start
set(VITA_APPNAME "Mad Bomber Vita")
set(VITA_TITLEID "BOMB00001")
set(VITA_VERSION "01.00")
set(PROJECT_NAME Mad_Bomber_Vita)

project(${PROJECT_NAME})
include("${VITASDK}/share/vita.cmake" REQUIRED)

# Flags and includes
set(DATA_PREFIX "app0:data/")
set(NOSOUNDFLAG __SOUND)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fno-exceptions")

add_definitions(-DVITA)
add_definitions(-D${NOSOUNDFLAG})
add_definitions(-DDATA_PREFIX=\"${DATA_PREFIX}\")

include_directories(
)

link_directories(
  ${CMAKE_CURRENT_BINARY_DIR}
)

# Builds
add_executable(${PROJECT_NAME}
madbomber.c
)

target_link_libraries(${PROJECT_NAME}
  SDL_mixer
  SDL_image
  SDL
  vorbisidec
  stdc++
  mikmod
  vita2d
  m
  z
  SceAppUtil_stub
  SceAudio_stub
  SceCommonDialog_stub
  SceCtrl_stub
  SceDisplay_stub
  SceGxm_stub
  SceSysmodule_stub
  SceTouch_stub
  SceHID_stub
)

# Create Vita artifacts
vita_create_self(eboot.bin ${PROJECT_NAME} SAFE)
vita_create_vpk(${PROJECT_NAME}.vpk ${VITA_TITLEID} eboot.bin
  VERSION ${VITA_VERSION}
  NAME ${VITA_APPNAME}
  FILE data data
  FILE sce_sys sce_sys

)
