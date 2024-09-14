# JoyHallo: Digital human model for Mandarin

<br>
<div align='left'>
    <a href='https://fudan-generative-vision.github.io/hallo/#/'><img src='https://img.shields.io/badge/Project-HomePage-Green'></a>
    <a href='https://huggingface.co/spaces/jdh-algo/JoyHallo-v1/tree/main'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-Model-yellow'></a>
</div>
<br>

## 📖 简介

在语音驱动的视频生成领域，制作普通话视频面临着巨大的挑战。收集全面的普通话数据集很困难，而且与英语相比，普通话复杂的唇形使模型训练更加复杂。我们的研究涉及从京东健康公司的员工那里收集的 29 小时的普通话语音视频，制作了 jdh-Hallo 数据集。该数据集涵盖了广泛的年龄和说话风格，包括对话和专门的医学主题。为了将 JoyHallo 模型适配到普通话中，我们使用了 Chinese-wav2vec 2.0 模型进行音频特征嵌入。此外，我们通过集成交叉注意力机制增强了分层音频驱动视觉合成模块，该机制汇总了来自唇部、表情和姿势特征的信息。这种集成不仅提高了信息利用效率，而且还将推理速度提高了 14.3%。适度的信息耦合使模型能够更有效的学习面部特征之间的关系，解决面部不自然的问题。这些进步使得音频输入和视频输出之间的对齐更加精确，从而提高了生成视频的质量和真实感。值得一提的是，JoyHallo 保留了强大的英文视频生成能力，展现出优秀的跨语言生成能力。

## 🎬 视频-中文-女生

https://github.com/user-attachments/assets/389e053f-e0c4-433c-8c60-80f9181d3f9c

## 🎬 视频-中文-男生

https://github.com/user-attachments/assets/1694efd9-2577-4bb5-ada4-7aa711d016a6

## 🎬 视频-英文

https://github.com/user-attachments/assets/d6b2efea-be76-442e-a8aa-ea0eef8b5f12

## 🧳 框架

![Network](assets/network.png "Network")

## ⚙️ 下载

系统配置:

- Tested on Ubuntu 20.04, Cuda 11.3
- Tested GPUs: A100

环境配置:

```bash
# 1. 创建虚拟环境
conda create -n joyhallo python=3.10 -y
conda activate joyhallo

# 2. 安装依赖
pip install -r requirements.txt

# 3. 安装ffmpeg
sudo apt-get update  
sudo apt-get install ffmpeg -y
```

## 🎒 模型准备

### 1. 下载基础权重

通过下面命令下载基础权重:

```shell
git lfs install
git clone https://huggingface.co/fudan-generative-ai/hallo pretrained_models
```

### 2. 下载 chinese-wav2vec2-base 模型

通过下面命令下载 `chinese-wav2vec2-base`:

```shell
cd pretrained_models
git lfs install
git clone https://huggingface.co/TencentGameMate/chinese-wav2vec2-base 
```

### 3. 下载 JoyHallo 模型权重

为了方便下载，我们分别在 **Huggingface** 和 **京东云** 上传了模型权重。

|   模型   |   数据   |                              Huggingface                              |                                        京东云                                        |        描述        |
| :------: | :-------: | :-------------------------------------------------------------------: | :----------------------------------------------------------------------------------: | :----------------: |
| JoyHallo | jdh-hallo | [JoyHallo](https://huggingface.co/spaces/jdh-algo/JoyHallo-v1/tree/main) | [JoyHallo](https://medicine-ai.s3.cn-north-1.jdcloud-oss.com/JoyHallo/joyhallo/net.pth) | 适用于JoyHallo模型 |
| ch-Hallo | jdh-hallo | [ch-Hallo](https://huggingface.co/spaces/jdh-algo/JoyHallo-v1/tree/main) | [ch-Hallo](https://medicine-ai.s3.cn-north-1.jdcloud-oss.com/JoyHallo/ch-hallo/net.pth) |  适用于Hallo模型  |

### 4. pretrained_models 目录

最后的 `pretrained_models` 文件夹目录如下:

```text
./pretrained_models/
|-- audio_separator/
|   |-- download_checks.json
|   |-- mdx_model_data.json
|   |-- vr_model_data.json
|   `-- Kim_Vocal_2.onnx
|-- ch-hallo/
|   `-- net.pth
|-- face_analysis/
|   `-- models/
|       |-- face_landmarker_v2_with_blendshapes.task
|       |-- 1k3d68.onnx
|       |-- 2d106det.onnx
|       |-- genderage.onnx
|       |-- glintr100.onnx
|       `-- scrfd_10g_bnkps.onnx
|-- hallo/
|   `-- net.pth
|-- joyhallo/
|   `-- net.pth
|-- motion_module/
|   `-- mm_sd_v15_v2.ckpt
|-- sd-vae-ft-mse/
|   |-- config.json
|   `-- diffusion_pytorch_model.safetensors
|-- stable-diffusion-v1-5/
|   `-- unet/
|       |-- config.json
|       `-- diffusion_pytorch_model.safetensors
|-- wav2vec/
|   `-- wav2vec2-base-960h/
|       |-- config.json
|       |-- feature_extractor_config.json
|       |-- model.safetensors
|       |-- preprocessor_config.json
|       |-- special_tokens_map.json
|       |-- tokenizer_config.json
|       `-- vocab.json
`-- chinese-wav2vec2-base/
    |-- chinese-wav2vec2-base-fairseq-ckpt.pt
    |-- config.json
    |-- preprocessor_config.json
    `-- pytorch_model.bin
```

## 🚧 数据要求

图片:

- 裁剪成方形；
- 人脸尽量向前，并且面部区域占比 `50%-70%`。

音频:

- 音频为 `wav`格式；
- 中文或者英语，音频尽量清晰，背景音乐适合。

注意：这里的要求**同时针对训练过程和推理过程**。

## 🚀 推理

使用下面命令进行推理:

```bash
sh joyhallo-infer.sh
```

修改 `configs/inference/inference.yaml` 中的参数为你想使用的音频和图像，以及切换模型，推理结果保存在 `opts/joyhallo`，`inference.yaml` 参数说明:

- audio_ckpt_dir: 模型权重路径；
- ref_img_path: 参考图片路径；
- audio_path: 参考音频路径；
- output_dir: 输出路径；
- exp_name: 输出文件夹。

## ⚓️ 训练或者微调 JoyHallo

训练或者微调模型时，你有两个选择：从 **1阶段** 开始训练或者**只训练 2阶段**。

### 1. 使用下面命令从 1阶段 开始训练

```
sh joyhallo-alltrain.sh
```

其会自动开始训练两个阶段（包含1阶段和2阶段），参考 `configs/train/stage1_alltrain.yaml`和 `configs/train/stage2_alltrain.yaml`调整训练参数。

### 2. 使用下面命令训练 2阶段

```
sh joyhallo-train.sh
```

其从 **2阶段** 开始训练，参考 `configs/train/stage2.yaml`调整训练参数。

## 🎓 准备训练数据

### 1. 按照下列目录准备数据，注意数据要符合前面提到的要求

```text
joyhallo/
|-- videos/
|   |-- 0001.mp4
|   |-- 0002.mp4
|   |-- 0003.mp4
|   `-- 0004.mp4
```

### 2. 使用下面命令处理数据集

```bash
python -m scripts.data_preprocess --input_dir joyhallo/videos --step 1
python -m scripts.data_preprocess --input_dir joyhallo/videos --step 2
```

## 💻 模型对比

### 1. 中文场景精度对比

|   模型   | Sync-C $\uparrow$ | Sync-D $\downarrow$ | Smooth $\uparrow$ | Subject $\uparrow$ | Background $\uparrow$ |
| :------: | :----------------: | :------------------: | :----------------: | :-----------------: | :--------------------: |
|  Hallo  |       5.7420       |  **13.8140**  |       0.9924       |       0.9855       |    **0.9651**    |
| JoyHallo |  **6.1596**  |       14.2053       |  **0.9925**  |  **0.9864**  |         0.9627         |

注意：这里评估使用的指标分别来自以下仓库，测评结果仅供对比参考：

- Sync-C 和 Sync-D: [Syncnet](https://github.com/joonson/syncnet_python)
- Smooth、Subject 和 Background: [VBench](https://github.com/Vchitect/VBench)

### 2. 推理效率对比

|                          | JoyHallo | Hallo |      提升      |
| :----------------------: | :------: | :----: | :-------------: |
| 显存（512*512，step 40） |  19049m  | 19547m | **2.5%** |
|   推理速度（16帧用时）   |   24s   |  28s  | **14.3%** |

## 📝 引用本项目

如果您觉得我们的工作有帮助，请考虑引用我们：

```
@misc{JoyHallo2024,
  title={JoyHallo: Digital human model for Mandarin},
  author={Sheng Shi and Xuyang Cao and Jun Zhao and Guoxin Wang},
  year={2024},
  url={https://huggingface.co/spaces/jdh-algo/JoyHallo-v1}
}
```

## 🤝 致谢

感谢这些项目的参与人员贡献了非常棒的开源工作：[Hallo](https://github.com/fudan-generative-vision/hallo)、[wav2vec 2.0](https://github.com/facebookresearch/fairseq/tree/main/examples/wav2vec)、[Chinese-wav2vec2](https://github.com/TencentGameMate/chinese_speech_pretrain)、[Syncnet](https://github.com/joonson/syncnet_python)、[VBench](https://github.com/Vchitect/VBench) 和 [Moore-AnimateAnyone](https://github.com/MooreThreads/Moore-AnimateAnyone)。