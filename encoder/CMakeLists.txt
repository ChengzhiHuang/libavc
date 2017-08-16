SET (CMAKE_C_FLAGS " -DNDEBUG -UHP_PL -DN_MB_ENABLE -fPIC -O3 ") 

SET (CMAKE_C_FALGS_x86 " -DX86 -msse4.2 ")

SET (libavc_top_srcdir ..)

SET (avcenc_src_files_common
	../common/ih264_resi_trans_quant.c
        ../common/ih264_iquant_itrans_recon.c
        ../common/ih264_ihadamard_scaling.c
        ../common/ih264_inter_pred_filters.c
        ../common/ih264_luma_intra_pred_filters.c
        ../common/ih264_chroma_intra_pred_filters.c
        ../common/ih264_padding.c
        ../common/ih264_mem_fns.c
        ../common/ih264_deblk_edge_filters.c
        ../common/ih264_deblk_tables.c
        ../common/ih264_cavlc_tables.c
        ../common/ih264_cabac_tables.c
        ../common/ih264_common_tables.c
        ../common/ih264_trans_data.c
        ../common/ih264_buf_mgr.c
        ../common/ih264_dpb_mgr.c
        ../common/ih264_list.c
        ../common/ithread.c
        ih264e_globals.c
        ih264e_intra_modes_eval.c
        ih264e_half_pel.c
        ih264e_mc.c
        ih264e_me.c
        ih264e_rc_mem_interface.c
        ih264e_time_stamp.c
        ih264e_modify_frm_rate.c
        ih264e_rate_control.c
        ih264e_core_coding.c
        ih264e_deblk.c
        ih264e_api.c
        ih264e_process.c
        ih264e_encode.c
        ih264e_utils.c
        ih264e_version.c
        ih264e_bitstream.c
        ih264e_cavlc.c
        ih264e_cabac_init.c
        ih264e_cabac.c
        ih264e_cabac_encode.c
        ih264e_encode_header.c
        ih264e_function_selector_generic.c
        ih264e_fmt_conv.c
        irc_rate_control_api.c
        irc_bit_allocation.c
        irc_cbr_buffer_control.c
        irc_est_sad.c
        irc_fixed_point_error_bits.c
        irc_frame_info_collector.c
        irc_mb_model_based.c
        irc_picture_type.c
        irc_rd_model.c
        irc_vbr_storage_vbv.c
        irc_vbr_str_prms.c
        ime.c
        ime_distortion_metrics.c
)

SET (avcenc_src_files_x86
	x86/ih264e_function_selector.c
        x86/ih264e_function_selector_sse42.c
        x86/ih264e_function_selector_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_iquant_itrans_recon_ssse3.c
	${libavc_top_srcdir}/common/x86/ih264_iquant_itrans_recon_dc_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_ihadamard_scaling_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_inter_pred_filters_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_mem_fns_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_padding_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_luma_intra_pred_filters_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_chroma_intra_pred_filters_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_deblk_chroma_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_deblk_luma_ssse3.c
        ${libavc_top_srcdir}/common/x86/ih264_iquant_itrans_recon_sse42.c
        ${libavc_top_srcdir}/common/x86/ih264_ihadamard_scaling_sse42.c
        ${libavc_top_srcdir}/common/x86/ih264_resi_trans_quant_sse42.c
        ${libavc_top_srcdir}/common/x86/ih264_weighted_pred_sse42.c
        x86/ih264e_half_pel_ssse3.c
        x86/ih264e_intra_modes_eval_ssse3.c
        x86/ime_distortion_metrics_sse42.c
)

INCLUDE_DIRECTORIES(
	${libavc_top_srcdir}/encoder
	${libavc_top_srcdir}/encoder/x86
	${libavc_top_srcdir}/common
	${libavc_top_srcdir}/common/x86
)

ADD_DEFINITIONS (${CMAKE_C_FLAGS} ${CMAKE_C_FALGS_x86})

ADD_LIBRARY (avcenc STATIC ${avcenc_src_files_common} ${avcenc_src_files_x86})

SET(LIBRARY_OUTPUT_PATH ${libavc_top_srcdir}/lib)
