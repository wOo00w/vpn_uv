# cmake verson
cmake_minimum_required(VERSION 3.2)

# project name
PROJECT(vpn_uv)

add_subdirectory("common/snappy")

# head file path
INCLUDE_DIRECTORIES(
common
common/libuv/
common/libuv/include
common/libuv/src
)

AUX_SOURCE_DIRECTORY(common/libuv/src LIB_UV_DIR_SRCS)

if(MSVC)
	AUX_SOURCE_DIRECTORY(common/libuv/src/win LIB_UV_PLATFORM_DIR_SRCS)
else()
	AUX_SOURCE_DIRECTORY(common/libuv/src/unix LIB_UV_PLATFORM_DIR_SRCS)
endif()

AUX_SOURCE_DIRECTORY(common/net_uv/base NET_UV_BASE_DIR_SRCS)
AUX_SOURCE_DIRECTORY(common/net_uv/kcp NET_UV_KCP_DIR_SRCS)
AUX_SOURCE_DIRECTORY(common/net_uv/puretcp LIB_UV_PURETCP__DIR_SRCS)
AUX_SOURCE_DIRECTORY(common/net_uv/tcp NET_UV_TCP_DIR_SRCS)
AUX_SOURCE_DIRECTORY(common/net_uv/udp NET_UV_UDP_DIR_SRCS)

AUX_SOURCE_DIRECTORY(common COMMON_DIR_SRCS)

AUX_SOURCE_DIRECTORY(server DIR_SRCS)

SET(VPN_UV_SRCS
${LIB_UV_DIR_SRCS}
${LIB_UV_PLATFORM_DIR_SRCS}
${NET_UV_BASE_DIR_SRCS}
${NET_UV_KCP_DIR_SRCS}
${LIB_UV_PURETCP__DIR_SRCS}
${NET_UV_TCP_DIR_SRCS}
${NET_UV_UDP_DIR_SRCS}
${COMMON_DIR_SRCS}
${DIR_SRCS}
)

ADD_EXECUTABLE(${PROJECT_NAME} ${VPN_UV_SRCS})

TARGET_LINK_LIBRARIES(${PROJECT_NAME} snappy)
