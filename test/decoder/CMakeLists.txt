SET (CMAKE_C_FLAGS " -DPROFILE_ENABLE -DARM -fPIC -DMD5_DISABLE ")

SET (libavcdec_top_srcdir ../..)

SET (avctest_src_files
	main.c
)

INCLUDE_DIRECTORIES(
	${libavcdec_top_srcdir}/decoder
	${libavcdec_top_srcdir}/common
)

LINK_DIRECTORIES (
	${libavcdec_top_srcdir}/lib
)

ADD_DEFINITIONS (${CMAKE_C_FLAGS})

ADD_EXECUTABLE (avcdecode ${avctest_src_files})

TARGET_LINK_LIBRARIES (avcdecode avcdec pthread)

SET (EXECUTABLE_OUTPUT_PATH ${libavcdec_top_srcdir})
