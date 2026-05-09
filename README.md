# AFNI-fMRI-data-analysis-2subs-try
# 先做预处理，时间矫正、头动矫正、生理噪声矫正（Fultz同时使用呼吸带和心电图，而只有fMRI数据的时，只能做头动矫正？）

[01_Fultz_Preproc_Step1.sh](https://github.com/user-attachments/files/27549210/01_Fultz_Preproc_Step1.sh)
#!/bin/bash
# ---------------------------------------------
# Script: 01_Fultz_Preproc_Step1.sh
# Purpose: Phase 1 Preprocessing (tshift, align, volreg, mask, regress)
# Reference: Fultz et al. (2019) Science 
# ---------------------------------------------
# 1. 进入工作目录
cd /mnt/e/data/PREVENT_fMRI_RAW_try
mkdir -p Analysis_Fultz
# 2. 阶段一脚本
for subj in WL001 WL002; do
    echo "======================================"
    echo "[阶段一] 启动被试 ${subj} 的 Fultz 严苛预处理..."
    fmri_file="./Resting/RS_MR_${subj}_v1.nii"
    anat_file="./mprage/T1w_${subj}A.nii"
    
    afni_proc.py -subj_id ${subj}_Fultz \
        -dsets ${fmri_file} \
        -copy_anat ${anat_file} \
        -blocks tshift align volreg mask regress \
        -regress_motion_per_run \
        -out_dir Analysis_Fultz/${subj}_Fultz.results \
        -execute
done 2>&1 | tee Analysis_Fultz/run_log_Step1_Preproc.txt
echo "阶段一：预处理完成"
