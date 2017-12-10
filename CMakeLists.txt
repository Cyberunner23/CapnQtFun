cmake_minimum_required(VERSION 3.0)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.19.188.tar.gz"
    SHA1 "67ab3a0206d8cdb9372ce22dc28eafc1d6bcc557"
)

project(CapnQtFun VERSION 0.0.1)

#capnproto
hunter_add_package(CapnProto)
find_package(CapnProto CONFIG REQUIRED)

set(CAPNP_GENERATED_DIR ${CMAKE_CURRENT_VINARY_DIR}/capnp)
set(CAPNP_FILES
    capnp/test.capnp
)

capnp_generate_cpp(CAPNP_SRCS CAPNP_HDRS 
    ${CAPNP_FILES}
)

foreach(file ${CAPNP_FILES})
    set_property(SOURCE "${CMAKE_CURRENT_BINARY_DIR}/${file}.c++" PROPERTY SKIP_AUTOMOC ON)
endforeach(file ${CAPNP_FILES})


#Qt
find_package(Qt5Core REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(CMAKE_AUTOMOC ON)


#Sources
set(Sources
    src/main.cpp
)

set(Headers

)


#Targets
add_executable(CapnQtFun ${Sources} ${Headers} ${CAPNP_SRCS} ${CAPNPC_HDRS})
target_link_libraries(CapnQtFun CapnProto::capnp Qt5::Core Qt5::Widgets)
target_include_directories(CapnQtFun PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/capnp)

