# 骨架识别/姿态识别
示例：[ANIMATED DRAWINGS](https://sketch.metademolab.com/)<br>
思路：人体关键点识别+2D转3D<br>
## 步骤：
![image](http://git.juntongai.com/zhaoyu/AnimatedDrawings/raw/branch/master/image/1.png)
1.使用openpose获取图片中的人物骨骼信息 
- 输入：图片（JPG、JPEG、PNG...）
- 输出：包含人物骨骼信息的json文件

2.使用pifuhd生成人物模型
- 输入：图片（JPG、JPEG、PNG...）+ 第一步中生成的json文件
- 输出：人物模型文件（obj）+ 效果图

3.使用Mixamo网站让生成的人物模型动起来<br>
## 1.Openpose
### 测试环境
windows10/11
### 下载
各版本下载[地址](https://github.com/CMU-Perceptual-Computing-Lab/openpose/releases)
<br>

测试版本为[1.6.0的gpu版本](https://github.com/CMU-Perceptual-Computing-Lab/openpose/releases/download/v1.6.0/openpose-1.6.0-binaries-win64-gpu-flir-3d_recommended.zip)，若需cpu版本，请[下载](https://github.com/CMU-Perceptual-Computing-Lab/openpose/releases/download/v1.6.0/openpose-1.6.0-binaries-win64-only_cpu-flir-3d.zip)

`./models`路径下双击`getModels.bat`文件，等待其自动下载后完成配置
### 运行
1.在`./input`或者自定义的路径下放入人物图片

2.在cmd中进入到openpose的路径：
```sh
cd XXX\...\AnimatedDrawings\openpose_1.6.0
```
3.运行以下代码：
```sh
bin\OpenPoseDemo.exe --image_dir 你的\图片\路径 --face --hand --write_json output/
```
其中`--image_dir`后面的参数为输入图片的路径，需要注意的是路径是图片文件的上一级，例如`...\...\input`，而不是单个图片的路径。因此支持批量输入。

`--write_json`后面的参数为输出路径，例如`output/`，输出的格式为包含人物骨骼点信息的json文件
### 注意
为了保证后续人物模型的生成效果，人物图片的选择建议为单人且无肢体遮挡的
## 2.Pifuhd
![Teaser Image](https://shunsukesaito.github.io/PIFuHD/resources/images/pifuhd.gif)
### 测试环境
- Python 3.8 
- [PyTorch](https://pytorch.org/) 1.11.0
- json
- PIL
- skimage
- tqdm
- cv2
- [trimesh](https://trimsh.org/) with pyembree
- PyOpenGL
- freeglut (use `sudo apt-get install freeglut3-dev` for ubuntu users)
- ffmpeg

注意: 建议至少预留8GB的GPU内存来运行Pifuhd模型。 

运行以下代码进行快速配置：
```sh
pip install -r requirements.txt 
```
### 下载预训练模型

运行以下代码下载预训练模型，保存在`./checkpoints/`路径下.
```
sh ./scripts/download_trained_model.sh
```

### 运行

运行以下代码进行快速测试：
```
python -m apps.simple_test
```
默认的输入路径：`./sample_images`<br>
输入：图片+该图片对应的json文件（注意保持同名）

默认的输出路径：`./results/pifuhd_final/recon`<br>
输出：人物模型文件+效果图

![image](http://git.juntongai.com/zhaoyu/AnimatedDrawings/raw/branch/master/image/result_test_512.png)

可在`./apps/simple_test.py`中修改参数：
```
--input_path  输入路径
--out_path    输出路径
--ckpt_path   模型路径
```
## 3.[Mixamo](https://www.mixamo.com/#/)
### 上传模型
进入网站并完成账号注册后，在网页右侧点击`"UPLOAD CHARACTER"`按钮，选择第二步中Pifuhd生成的人物模型文件（obj后缀）并上传
### 绑定骨骼
仿照右侧的示例，绑定好模型的骨骼

![image](http://git.juntongai.com/zhaoyu/AnimatedDrawings/raw/branch/master/image/3.png)

下方可以选择不勾选 `Use Symmetry`
### 动起来
完成前两步后，在网页左侧的动作库中点击选择想要展示的动作
![image](http://git.juntongai.com/zhaoyu/AnimatedDrawings/raw/branch/master/image/2.png)
![image](http://git.juntongai.com/zhaoyu/AnimatedDrawings/raw/branch/master/image/%e5%8a%a8%e7%94%bb.gif)
