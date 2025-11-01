# --------------------------------------------------------------------------
# Dependencies
# --------------------------------------------------------------------------

find_package(yat CONFIG REQUIRED)
find_package(pylon CONFIG REQUIRED)

# --------------------------------------------------------------------------
# Collect sources and includes
# --------------------------------------------------------------------------

file(GLOB_RECURSE sources src/*.cpp)
set(includedirs
    include/
)

# --------------------------------------------------------------------------
# Target
# --------------------------------------------------------------------------

add_library(limabasler ${sources})
target_include_directories(limabasler 
    PUBLIC 
        ${includedirs}
)
target_link_libraries(limabasler 
    PUBLIC 
        limacore
        yat::yat
        pylon::pylon
)

limatools_set_library_soversion(limabasler "VERSION")

# --------------------------------------------------------------------------
# Compile definitions
# --------------------------------------------------------------------------

file(READ "${CMAKE_CURRENT_SOURCE_DIR}/VERSION" BASLER_VERSION_RAW)
string(STRIP "${BASLER_VERSION_RAW}" BASLER_VERSION)

target_compile_definitions(limabasler
    PUBLIC
        BASLER_NAME=Basler
        BASLER_VERSION=${BASLER_VERSION}
        BASLER_ENABLED
)

message(STATUS "Camera enabled: Basler ${BASLER_VERSION}")

target_compile_definitions(limabasler
    PRIVATE
        USE_GIGE
        VIDEO_COLOR_DISABLED
)

target_compile_options(limabasler
    PRIVATE
        -fpermissive
)

# --------------------------------------------------------------------------
# Install
# --------------------------------------------------------------------------

install(
    TARGETS limabasler 
    LIBRARY DESTINATION lib
)
