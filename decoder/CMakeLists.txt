
SET (CMAKE_C_FLAGS " -fPIC -O3 -DX86 -msse4.2 -DDEFAULT_ARCH=D_ARCH_X86_SSE42 ") 

SET (libavcdec_top_srcdir ..)

SET (avcdec_src_files
	../common/ih264_buf_mgr.c
	../common/ih264_disp_mgr.c
	../common/ih264_inter_pred_filters.c
	../common/ih264_luma_intra_pred_filters.c
	../common/ih264_chroma_intra_pred_filters.c
	../common/ih264_padding.c
	../common/ih264_mem_fns.c
	../common/ih264_deblk_edge_filters.c
	../common/ih264_iquant_itrans_recon.c
	../common/ih264_ihadamard_scaling.c
	../common/ih264_weighted_pred.c
	../common/ithread.c
	ih264d_cabac.c
	ih264d_parse_mb_header.c
	ih264d_parse_cabac.c
	ih264d_process_intra_mb.c
	ih264d_inter_pred.c
	ih264d_parse_bslice.c
	ih264d_parse_pslice.c
	ih264d_parse_islice.c
	ih264d_cabac_init_tables.c
	ih264d_bitstrm.c
	ih264d_compute_bs.c
	ih264d_deblocking.c
	ih264d_parse_headers.c
	ih264d_mb_utils.c
	ih264d_mvpred.c
	ih264d_utils.c
	ih264d_process_bslice.c
	ih264d_process_pslice.c
	ih264d_parse_slice.c
	ih264d_quant_scaling.c
	ih264d_parse_cavlc.c
	ih264d_dpb_mgr.c
	ih264d_nal.c
	ih264d_sei.c
	ih264d_tables.c
	ih264d_vui.c
	ih264d_format_conv.c
	ih264d_thread_parse_decode.c
	ih264d_api.c
	ih264d_thread_compute_bs.c
	ih264d_function_selector_generic.c

	x86/ih264d_function_selector.c
	x86/ih264d_function_selector_sse42.c
	x86/ih264d_function_selector_ssse3.c
	../common/x86/ih264_inter_pred_filters_ssse3.c
	../common/x86/ih264_deblk_luma_ssse3.c
	../common/x86/ih264_deblk_chroma_ssse3.c
	../common/x86/ih264_padding_ssse3.c
	../common/x86/ih264_mem_fns_ssse3.c
	../common/x86/ih264_iquant_itrans_recon_dc_ssse3.c
	../common/x86/ih264_iquant_itrans_recon_ssse3.c
	../common/x86/ih264_luma_intra_pred_filters_ssse3.c
	../common/x86/ih264_chroma_intra_pred_filters_ssse3.c
	../common/x86/ih264_iquant_itrans_recon_sse42.c
	../common/x86/ih264_weighted_pred_sse42.c
	../common/x86/ih264_ihadamard_scaling_sse42.c
)
 
INCLUDE_DIRECTORIES(
	${libavcdec_top_srcdir}/decoder
	${libavcdec_top_srcdir}/common
	${libavcdec_top_srcdir}/common/x86
)

ADD_DEFINITIONS (${CMAKE_C_FLAGS})

ADD_LIBRARY (avcdec STATIC ${avcdec_src_files})

SET(LIBRARY_OUTPUT_PATH ${libavcdec_top_srcdir}/lib)