SET (CMAKE_C_FLAGS_TEST  "-DPROFILE_ENABLE -DARM -DMD5_DISABLE -fPIC -pie")

SET (libavc_top_srcdir ../../)

SET (avc_enctest_src_files
	main.c
	psnr.c
	input.c
	output.c
	recon.c
)
INCLUDE_DIRECTORIES(
	${libavc_top_srcdir}/test/encoder
	${libavc_top_srcdir}/common
	${libavc_top_srcdir}/encoder
	/usr/include #math.h
)

LINK_DIRECTORIES (
	${libavc_top_srcdir}/lib
	/usr/lib/libx86_64-linux-gnu #libm.so, 
)

ADD_DEFINITIONS (${CMAKE_C_FLAGS_TEST})

ADD_EXECUTABLE (avcencode ${avc_enctest_src_files})

TARGET_LINK_LIBRARIES (avcencode avcenc pthread m)

SET (EXECUTABLE_OUTPUT_PATH ${libavc_top_srcdir}/bin)
