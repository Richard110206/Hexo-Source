---
title: ASC26
date: 2026-02-27 01:15:51
tags: [review]
category: Mathematical Modeling Tutorial
category_bar: true
archive: true
---

- `sinfo`可以看到当前的分区状况
  - `PARTITION`	节点所属的分区（* 表示这是默认分区）
  - `AVAIL`	分区是否可用
  - `TIMELIMIT`	作业运行的时间限制
  - `NODES`	对应状态的节点数量
  - `STATE`	节点状态
    - `down*`：节点处于下线 / 不可用状态（* 通常表示管理员手动下线）
    - `mix`：节点部分资源被占用（有作业在运行，但仍有空闲资源可提交新作业）
    - `alloc`：节点资源已全部分配（满负载，无空闲资源）
    - `idle`：节点空闲（无作业运行，资源完全可用）
  - `NODELIST`	具体的节点名称列表
```bash
ASC1598@wx-ms-w7900d-0054:~$ sinfo
PARTITION      AVAIL  TIMELIMIT  NODES  STATE NODELIST
gpu_partition*    up   infinite      2  down* wx-ms-w7900d-[0033-0034]
gpu_partition*    up   infinite      2    mix wx-ms-w7900d-[0032,0039]
gpu_partition*    up   infinite      1  alloc wx-ms-w7900d-0035
gpu_partition*    up   infinite      3   idle wx-ms-w7900d-[0036-0038]
```
- 通过`srun`命令进入计算节点
```bash
ASC1598@wx-ms-w7900d-0054:~$ srun -p gpu_partition -w wx-ms-w7900d-0038 -N 1 -n 1 --gres=gpu:1 --pty /bin/bash
ASC1598@wx-ms-w7900d-0038:
```

- 接下来确认GPU是否可用
```bash
ASC1598@wx-ms-w7900d-0038:~$ rocm-smi


WARNING: AMD GPU device(s) is/are in a low-power state. Check power control/runtime_status

======================================== ROCm System Management Interface ========================================
================================================== Concise Info ==================================================
Device  Node  IDs              Temp    Power  Partitions          SCLK  MCLK   Fan    Perf  PwrCap  VRAM%  GPU%
              (DID,     GUID)  (Edge)  (Avg)  (Mem, Compute, ID)
==================================================================================================================
0       2     0x744b,   6853   49.0°C  70.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
1       3     0x744b,   49884  32.0°C  19.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
2       4     0x744b,   60148  24.0°C  12.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
3       5     0x744b,   13037  25.0°C  12.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
4       6     0x744b,   47780  29.0°C  17.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
5       7     0x744b,   25277  25.0°C  12.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
6       8     0x744b,   19093  28.0°C  14.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
7       9     0x744b,   37516  32.0°C  15.0W  N/A, N/A, 0         0Mhz  96Mhz  20.0%  auto  241.0W  0%     0%
==================================================================================================================
============================================== End of ROCm SMI Log ===============================================
```

- 分别尝试使用`http`和`ssh`的方法进行下载均以失败告终，最终通过`wget`下载成功
```bash
ASC1598@wx-ms-w7900d-0038:~/ASC26/Task3_dev/unifolm-world-model-action/checkpoints$ wget -O unifolm_wma_dual.ckpt https://hf-mirror.com/unitreerobotics/UnifoLM-WMA-0-Dual/resolve/main/unifolm_wma_dual.ckpt
--2026-02-27 03:50:05--  https://hf-mirror.com/unitreerobotics/UnifoLM-WMA-0-Dual/resolve/main/unifolm_wma_dual.ckpt
Resolving hf-mirror.com (hf-mirror.com)... 160.16.86.14
Connecting to hf-mirror.com (hf-mirror.com)|160.16.86.14|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://cas-bridge.xethub.hf.co/xet-bridge-us/68ca20edb9011e89cbbd2b09/08532fdce27067e963ec5f326b312e8ef89e5e3e44dfcc4111abfc0aab7c706a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=cas%2F20260227%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260227T035005Z&X-Amz-Expires=3600&X-Amz-Signature=7adc6047de5563b0bfc289ed40f1961eac68d98141281df7b05d6180782776da&X-Amz-SignedHeaders=host&X-Xet-Cas-Uid=62171e3b6a99db28e0b3159d&response-content-disposition=inline%3B+filename*%3DUTF-8%27%27unifolm_wma_dual.ckpt%3B+filename%3D%22unifolm_wma_dual.ckpt%22%3B&x-id=GetObject&Expires=1772167805&Policy=eyJTdGF0ZW1lbnQiOlt7IkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc3MjE2NzgwNX19LCJSZXNvdXJjZSI6Imh0dHBzOi8vY2FzLWJyaWRnZS54ZXRodWIuaGYuY28veGV0LWJyaWRnZS11cy82OGNhMjBlZGI5MDExZTg5Y2JiZDJiMDkvMDg1MzJmZGNlMjcwNjdlOTYzZWM1ZjMyNmIzMTJlOGVmODllNWUzZTQ0ZGZjYzQxMTFhYmZjMGFhYjdjNzA2YSoifV19&Signature=o%7EVjgrlbQQj1%7EoVLL0m9NfXAs6FZhgRrxMgo43A6xpy0YBlLOkk%7EwLMtL1qyiim%7EJMvHMJrldvxMfnbRCLIx5T%7EIAYjNb23lYLYELAatR-2VdqK319a9W5rEamh7bt63nnLxeyh0FJbw99sLyOVc6gUiVs%7E8yAlkjXHWRYxKNaB%7E1w-u8R27ovW40VHsnl8kQO1VycWeoAdawkjU0t6NZjC2qOW7%7E0bCR3FyepzDNnhbfO5MRJtrBM4cZ7zisqEXyXCesFzg2Sy9fAGWS1f5zxMHmAHfgcKGh92UU7IjPaNQRlLTUelbYiVEssJNaDRUfnOQhO60aRxQFxDsbftU8Q__&Key-Pair-Id=K2L8F4GPSG1IFC [following]
--2026-02-27 03:50:06--  https://cas-bridge.xethub.hf.co/xet-bridge-us/68ca20edb9011e89cbbd2b09/08532fdce27067e963ec5f326b312e8ef89e5e3e44dfcc4111abfc0aab7c706a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=cas%2F20260227%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20260227T035005Z&X-Amz-Expires=3600&X-Amz-Signature=7adc6047de5563b0bfc289ed40f1961eac68d98141281df7b05d6180782776da&X-Amz-SignedHeaders=host&X-Xet-Cas-Uid=62171e3b6a99db28e0b3159d&response-content-disposition=inline%3B+filename*%3DUTF-8%27%27unifolm_wma_dual.ckpt%3B+filename%3D%22unifolm_wma_dual.ckpt%22%3B&x-id=GetObject&Expires=1772167805&Policy=eyJTdGF0ZW1lbnQiOlt7IkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc3MjE2NzgwNX19LCJSZXNvdXJjZSI6Imh0dHBzOi8vY2FzLWJyaWRnZS54ZXRodWIuaGYuY28veGV0LWJyaWRnZS11cy82OGNhMjBlZGI5MDExZTg5Y2JiZDJiMDkvMDg1MzJmZGNlMjcwNjdlOTYzZWM1ZjMyNmIzMTJlOGVmODllNWUzZTQ0ZGZjYzQxMTFhYmZjMGFhYjdjNzA2YSoifV19&Signature=o%7EVjgrlbQQj1%7EoVLL0m9NfXAs6FZhgRrxMgo43A6xpy0YBlLOkk%7EwLMtL1qyiim%7EJMvHMJrldvxMfnbRCLIx5T%7EIAYjNb23lYLYELAatR-2VdqK319a9W5rEamh7bt63nnLxeyh0FJbw99sLyOVc6gUiVs%7E8yAlkjXHWRYxKNaB%7E1w-u8R27ovW40VHsnl8kQO1VycWeoAdawkjU0t6NZjC2qOW7%7E0bCR3FyepzDNnhbfO5MRJtrBM4cZ7zisqEXyXCesFzg2Sy9fAGWS1f5zxMHmAHfgcKGh92UU7IjPaNQRlLTUelbYiVEssJNaDRUfnOQhO60aRxQFxDsbftU8Q__&Key-Pair-Id=K2L8F4GPSG1IFC
Resolving cas-bridge.xethub.hf.co (cas-bridge.xethub.hf.co)... 13.33.183.37, 13.33.183.46, 13.33.183.18, ...
Connecting to cas-bridge.xethub.hf.co (cas-bridge.xethub.hf.co)|13.33.183.37|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16719237688 (16G)
Saving to: ‘unifolm_wma_dual.ckpt’

unifolm_wma_dual.ckpt           100%[====================================================>]  15.57G   112MB/s    in 2m 50s

2026-02-27 03:52:56 (93.6 MB/s) - ‘unifolm_wma_dual.ckpt’ saved [16719237688/16719237688]
```