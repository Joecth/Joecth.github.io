---
layout: post
categories: DevOps
date: 2022-05-29
tag: [] 
---







```bash
	  2  sudo apt-get update
    3  wget https://developer.download.nvidia.com/compute/cuda/11.7.0/local_installers/cuda_11.7.0_515.43.04_linux.run	  
    6  sudo sh cuda_11.7.0_515.43.04_linux.run
   11  wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
   13  sh Miniconda3-py39_4.12.0-Linux-x86_64.sh
   16  vi ~/.bashrc	# LD_LIBRARY_PATH=/usr/local/cuda-11.7/lib64
   18  source ~/.bashrc
   19  conda create -n dl37 python=3.7 pip
   20  conda activate dl37
   21  pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113
   # 可看到 pytorch 2G，比 cuda還要大! 也是相當厲害…
   22  wget https://zh-v2.d2l.ai/d2l-zh.zip
   23  sudo apt-get install unzip
   33  pip install -U d2l jupyter
   34  jupyter notebook
```





## jupyter notebook

```bash
ssh -i ~/.ssh/emr-key-A.pem -L8888:localhost:8888 ubuntu@ec2-54-159-193-111.compute-1.amazonaws.com
```





## nvidia-smi

```shell
(base) ubuntu@ip-192-168-81-198:~$ nvidia-smi
Mon May 30 09:58:50 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.43.04    Driver Version: 515.43.04    CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            Off  | 00000000:00:1E.0 Off |                    0 |
| N/A   32C    P0    27W /  70W |   2751MiB / 15360MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     11902      C   ...nda3/envs/dl37/bin/python     2747MiB |
+-----------------------------------------------------------------------------+
(base) ubuntu@ip-192-168-81-198:~$ nvidia-smi
Mon May 30 09:59:31 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.43.04    Driver Version: 515.43.04    CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            Off  | 00000000:00:1E.0 Off |                    0 |
| N/A   33C    P0    27W /  70W |    335MiB / 15360MiB |      6%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     12017      C   ...nda3/envs/dl37/bin/python      331MiB |
+-----------------------------------------------------------------------------+
(base) ubuntu@ip-192-168-81-198:~$ nvidia-smi
Mon May 30 09:59:38 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.43.04    Driver Version: 515.43.04    CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            Off  | 00000000:00:1E.0 Off |                    0 |
| N/A   37C    P0    79W /  70W |   2749MiB / 15360MiB |     98%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     12017      C   ...nda3/envs/dl37/bin/python     2745MiB |
+-----------------------------------------------------------------------------+
(base) ubuntu@ip-192-168-81-198:~$ nvidia-smi
Mon May 30 09:59:42 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.43.04    Driver Version: 515.43.04    CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            Off  | 00000000:00:1E.0 Off |                    0 |
| N/A   38C    P0    68W /  70W |   2749MiB / 15360MiB |     97%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     12017      C   ...nda3/envs/dl37/bin/python     2745MiB |
+-----------------------------------------------------------------------------+
```





## InfoGPU

```python
import torch
torch.cuda.current_device()
torch.cuda.device_count()
torch.cuda.get_device_name(0)
torch.cuda.is_available()
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38arz9jvxj20u00ue42e.jpg" alt="image-20220530192309711" style="zoom:50%;" />

```python
>>> torch.version.cuda
'11.3'
>>>
```



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2qots3vvfj213c0ac3zt.jpg" alt="image-20220530200847113" style="zoom:50%;" />

​				

```python
dl37rmbg) ubuntu@ip-192-168-81-198:~/codes/rm-bg-test$ pip list | grep onnx
onnxruntime                  1.11.1
(dl37rmbg) ubuntu@ip-192-168-81-198:~/codes/rm-bg-test$
```





### Before

```shell
 * Debug mode: off
2022-05-30 12:01:59 INFO      * Running on all addresses (0.0.0.0)
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://127.0.0.1:5005
 * Running on http://192.168.81.198:5005 (Press CTRL+C to quit)
2022-05-30 12:02:06 INFO     49.216.44.45 - - [30/May/2022 12:02:06] "GET / HTTP/1.1" 200 -
2022-05-30 12:02:06 INFO     49.216.44.45 - - [30/May/2022 12:02:06] "GET /favicon.ico HTTP/1.1" 404 -
2022-05-30 12:02:14 INFO     49.216.44.45 - - [30/May/2022 12:02:14] "OPTIONS /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:02:15 INFO     Using algorithm -- u2
2022-05-30 12:02:15 INFO     in get_prediction!!!!
2022-05-30 12:02:15 INFO     dict_keys(['image'])
2022-05-30 12:02:15 INFO     Time spent for successfully uploaded usr img img4predict_20220530-12:02:15.jpeg to S3 : 0.7874946594238281
2022-05-30 12:02:15 INFO     Acquiring U2 Lock... Waiting...
2022-05-30 12:02:15 INFO     Time spent for Acquiring U2 Lock: 6.9141387939453125e-06
2022-05-30 12:02:15 INFO     Start predicting with U2
2022-05-30 12:02:15 INFO     Acquired Lock for U2!
2022-05-30 12:02:15 INFO     Time spent for reading image from S3 by cv2! : 0.06653761863708496
Downloading...
From: https://drive.google.com/uc?id=1tCU5MM1LhRgGou5OpmpjBQbSrYIUoYab
To: /home/ubuntu/.u2net/u2net.onnx
100%|████████████████████████████████████████████| 176M/176M [00:01<00:00, 104MB/s]
2022-05-30 12:02:19 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model.detect: 3.3960742950439453
2022-05-30 12:02:19 INFO     Time spent for U2: 3.4032671451568604
2022-05-30 12:02:19 INFO     uploaded_name: removed_back_U2_20220530-12:02:15.png
2022-05-30 12:02:19 INFO     Time spent for upload_file to S3: 0.09176993370056152
2022-05-30 12:02:19 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model: 3.5619876384735107
2022-05-30 12:02:19 INFO     Released Lock for U2!
2022-05-30 12:02:19 INFO     49.216.44.45 - - [30/May/2022 12:02:19] "POST /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:02:37 INFO     49.216.44.45 - - [30/May/2022 12:02:37] "OPTIONS /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:02:37 INFO     Using algorithm -- u2
2022-05-30 12:02:37 INFO     in get_prediction!!!!
2022-05-30 12:02:40 INFO     dict_keys(['image'])
2022-05-30 12:02:41 INFO     Time spent for successfully uploaded usr img img4predict_20220530-12:02:40.jpeg to S3 : 3.314974784851074
2022-05-30 12:02:41 INFO     Acquiring U2 Lock... Waiting...
2022-05-30 12:02:41 INFO     Time spent for Acquiring U2 Lock: 5.245208740234375e-06
2022-05-30 12:02:41 INFO     Start predicting with U2
2022-05-30 12:02:41 INFO     Acquired Lock for U2!
2022-05-30 12:02:41 INFO     Time spent for reading image from S3 by cv2! : 0.13377881050109863
2022-05-30 12:02:43 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model.detect: 2.55837082862854
2022-05-30 12:02:43 INFO     Time spent for U2: 2.6617114543914795
2022-05-30 12:02:43 INFO     uploaded_name: removed_back_U2_20220530-12:02:40.png
2022-05-30 12:02:44 INFO     Time spent for upload_file to S3: 0.1535935401916504
2022-05-30 12:02:44 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model: 2.9496042728424072
2022-05-30 12:02:44 INFO     Released Lock for U2!
2022-05-30 12:02:44 INFO     49.216.44.45 - - [30/May/2022 12:02:44] "POST /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:04:49 INFO     49.216.44.45 - - [30/May/2022 12:04:49] "OPTIONS /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:04:49 INFO     Using algorithm -- u2
2022-05-30 12:04:49 INFO     in get_prediction!!!!
2022-05-30 12:04:51 INFO     dict_keys(['image'])
2022-05-30 12:04:52 INFO     Time spent for successfully uploaded usr img img4predict_20220530-12:04:51.jpeg to S3 : 2.766263723373413
2022-05-30 12:04:52 INFO     Acquiring U2 Lock... Waiting...
2022-05-30 12:04:52 INFO     Time spent for Acquiring U2 Lock: 6.67572021484375e-06
2022-05-30 12:04:52 INFO     Start predicting with U2
2022-05-30 12:04:52 INFO     Acquired Lock for U2!
2022-05-30 12:04:52 INFO     Time spent for reading image from S3 by cv2! : 0.07762646675109863
2022-05-30 12:04:54 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model.detect: 1.9941890239715576
2022-05-30 12:04:54 INFO     Time spent for U2: 2.0525596141815186
2022-05-30 12:04:54 INFO     uploaded_name: removed_back_U2_20220530-12:04:51.png
2022-05-30 12:04:54 INFO     Time spent for upload_file to S3: 0.11681175231933594
2022-05-30 12:04:54 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model: 2.2475361824035645
2022-05-30 12:04:54 INFO     Released Lock for U2!
2022-05-30 12:04:54 INFO     49.216.44.45 - - [30/May/2022 12:04:54] "POST /predict/u2 HTTP/1.1" 200 -
```



### After

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2qoyzgd0cj20xo0d876d.jpg" alt="image-20220530201339682" style="zoom:50%;" />

```shell
 * Debug mode: off
2022-05-30 12:14:18 INFO      * Running on all addresses (0.0.0.0)
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://127.0.0.1:5005
 * Running on http://192.168.81.198:5005 (Press CTRL+C to quit)
2022-05-30 12:14:59 INFO     49.216.44.45 - - [30/May/2022 12:14:59] "OPTIONS /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:15:00 INFO     Using algorithm -- u2
2022-05-30 12:15:00 INFO     in get_prediction!!!!
2022-05-30 12:15:02 INFO     dict_keys(['image'])
2022-05-30 12:15:03 INFO     Time spent for successfully uploaded usr img img4predict_20220530-12:15:02.jpeg to S3 : 3.0525119304656982
2022-05-30 12:15:03 INFO     Acquiring U2 Lock... Waiting...
2022-05-30 12:15:03 INFO     Time spent for Acquiring U2 Lock: 6.198883056640625e-06
2022-05-30 12:15:03 INFO     Start predicting with U2
2022-05-30 12:15:03 INFO     Acquired Lock for U2!
2022-05-30 12:15:03 INFO     Time spent for reading image from S3 by cv2! : 0.10718274116516113
2022-05-30 12:15:03.755888640 [W:onnxruntime:Default, onnxruntime_pybind_state.cc:526 CreateExecutionProviderInstance] Failed to create TensorrtExecutionProvider. Please reference https://onnxruntime.ai/docs/execution-providers/TensorRT-ExecutionProvider.html#requirements to ensure all dependencies are met.
2022-05-30 12:15:03.755924162 [W:onnxruntime:Default, onnxruntime_pybind_state.cc:552 CreateExecutionProviderInstance] Failed to create CUDAExecutionProvider. Please reference https://onnxruntime.ai/docs/reference/execution-providers/CUDA-ExecutionProvider.html#requirements to ensure all dependencies are met.
2022-05-30 12:15:05 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model.detect: 2.503098249435425
2022-05-30 12:15:05 INFO     Time spent for U2: 2.5608794689178467
2022-05-30 12:15:05 INFO     uploaded_name: removed_back_U2_20220530-12:15:02.png
2022-05-30 12:15:05 INFO     Time spent for upload_file to S3: 0.0716698169708252
2022-05-30 12:15:05 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model: 2.740231990814209
2022-05-30 12:15:05 INFO     Released Lock for U2!
2022-05-30 12:15:05 INFO     49.216.44.45 - - [30/May/2022 12:15:05] "POST /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:15:50 INFO     49.216.44.45 - - [30/May/2022 12:15:50] "OPTIONS /predict/u2 HTTP/1.1" 200 -
2022-05-30 12:15:50 INFO     Using algorithm -- u2
2022-05-30 12:15:50 INFO     in get_prediction!!!!
2022-05-30 12:15:53 INFO     dict_keys(['image'])
2022-05-30 12:15:53 INFO     Time spent for successfully uploaded usr img img4predict_20220530-12:15:53.jpeg to S3 : 2.71683931350708
2022-05-30 12:15:53 INFO     Acquiring U2 Lock... Waiting...
2022-05-30 12:15:53 INFO     Time spent for Acquiring U2 Lock: 6.67572021484375e-06
2022-05-30 12:15:53 INFO     Start predicting with U2
2022-05-30 12:15:53 INFO     Acquired Lock for U2!
2022-05-30 12:15:53 INFO     Time spent for reading image from S3 by cv2! : 0.09477829933166504
2022-05-30 12:15:54.261673705 [W:onnxruntime:Default, onnxruntime_pybind_state.cc:526 CreateExecutionProviderInstance] Failed to create TensorrtExecutionProvider. Please reference https://onnxruntime.ai/docs/execution-providers/TensorRT-ExecutionProvider.html#requirements to ensure all dependencies are met.
2022-05-30 12:15:54.261700073 [W:onnxruntime:Default, onnxruntime_pybind_state.cc:552 CreateExecutionProviderInstance] Failed to create CUDAExecutionProvider. Please reference https://onnxruntime.ai/docs/reference/execution-providers/CUDA-ExecutionProvider.html#requirements to ensure all dependencies are met.
2022-05-30 12:15:55 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model.detect: 1.9362876415252686
2022-05-30 12:15:55 INFO     Time spent for U2: 2.035747528076172
2022-05-30 12:15:55 INFO     uploaded_name: removed_back_U2_20220530-12:15:53.png
2022-05-30 12:15:56 INFO     Time spent for upload_file to S3: 0.16250967979431152
2022-05-30 12:15:56 INFO     >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> Time spent for U2 model: 2.2935831546783447
2022-05-30 12:15:56 INFO     Released Lock for U2!
2022-05-30 12:15:56 INFO     49.216.44.45 - - [30/May/2022 12:15:56] "POST /predict/u2 HTTP/1.1" 200 -

```

#### To be Debugged...

```shell
(dl37) ubuntu@ip-192-168-81-198:~/codes/rm-bg-test$ nvcc
Command 'nvcc' not found, but can be installed with:
sudo apt install nvidia-cuda-toolkit
(dl37) ubuntu@ip-192-168-81-198:~/codes/rm-bg-test$ sudo apt install nvidia-cuda-toolkit
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2qwhz0066j21bg0r6wk6.jpg" alt="image-20220531003424795" style="zoom:50%;" />



still issue:

![image-20220531011651588](https://tva1.sinaimg.cn/large/e6c9d24egy1h2qxq59vu7j21l60u0q9d.jpg)





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2ribc6nnkj21jl0u0tdh.jpg" alt="image-20220531130858981" style="zoom:80%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2rrsnyp4cj22280pgtev.jpg" alt="image-20220531183707169" style="zoom:67%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38as3hkw7j20zq03swet.jpg" alt="image-20220601201815869" style="zoom:70%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38as53kxxj21om0te0z5.jpg" alt="image-20220602130731483" style="zoom:67%;" />





### Fixed!

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2tuku93g7j21a70u0n2c.jpg" alt="image-20220602134439595"  />



### T4: 8TFLOPS

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38as95071j20ws0f80tt.jpg" alt="image-20220530175459347" style="zoom:50%;" />





vs 



## Desktop

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38asbbp5qj21060u041a.jpg" alt="image-20220530190225031" style="zoom:67%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2qmxf3k38j213g0mmwg7.jpg" alt="image-20220530190314249" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38asf5hnbj215e0l4q7e.jpg" alt="image-20220530190425812" style="zoom:67%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38ash054dj20zy0l6juc.jpg" alt="image-20220530190343502" style="zoom:67%;" />





## Others 白嫖平台

Colab

Kabble

AI Studio

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h2qn0jg4rhj21090u0din.jpg" alt="image-20220530190615177" style="zoom:67%;" />







ref: https://www.bilibili.com/video/BV1MA411L78X?t=493







## CUDAs

2







`torch.version.cuda` is just defined as a string. It doesn't query anything. It doesn't tell you which version of CUDA you have installed. It only tells you that the PyTorch you have installed is meant for that (`10.2`) version of CUDA. But the version of CUDA you are actually running on your system is `11.4`.

If you installed PyTorch with, say,

```py
conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c nvidia
```

then you should also have the necessary libraries (`cudatoolkit`) in your Anaconda directory, which may be different from your system-level libraries.

However, note that these depend on the NVIDIA display drivers:

[![enter image description here](https://tva1.sinaimg.cn/large/e6c9d24egy1h2qxb8m3snj20dw0auwf5.jpg)](https://i.stack.imgur.com/lKSLg.png)

Installing `cudatoolkit` does not install the drivers (`nvidia.ko`), which you need to install separately on your system.



https://stackoverflow.com/questions/69497328/why-are-torch-version-cuda-and-devicequery-reporting-different-versions

