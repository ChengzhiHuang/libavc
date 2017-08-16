SET (CMAKE_C_FLAGS " -DPROFILE_ENABLE -DARM -fPIC -DMD5_DISABLE ")

SET (libavc_top_srcdir ../..)

SET (avc_dectest_src_files
	main.c
)

INCLUDE_DIRECTORIES(
	${libavc_top_srcdir}/decoder
	${libavc_top_srcdir}/common
)

LINK_DIRECTORIES (
	${libavcdec_top_srcdir}/lib
)

ADD_DEFINITIONS (${CMAKE_C_FLAGS})

ADD_EXECUTABLE (avcdecode ${avc_dectest_src_files})

TARGET_LINK_LIBRARIES (avcdecode avcdec pthread)

SET (EXECUTABLE_OUTPUT_PATH ${libavc_top_srcdir}/bin)
