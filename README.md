
关于本项目
================================

## 前言

运行该项目中的代码需配置好[manim](https://github.com/3b1b/manim)才能运行，[我用的manim版本](https://github.com/cigar666/manim)在[3b1b的manim](https://github.com/3b1b/manim)的基础上略有改动。这是一些练习manim的demo。

### 依赖环境：

manim是制作数学动画的引擎。本项目依赖环境包括 ：

1. python 3.8 或更新的版本
2. FFmpeg
3. sox
4. TeX宏集环境
5. 一些必要的python包

### 安装

官方安装方式，但是安装失败，编码有问题：

```sh
pip install manimlib
```

偶然使用下面的方法安装成功，并且可以正常使用：

```sh
pip install manim lib
```

安装后便可以通过如下方式使用manim命令行

```sh
manim my_project.py MyScene
```

更多选项可查看 [Using manim](#using-manim) 。

### 直接使用

如果您想使用manimlib本身，克隆存储库https://github.com/3b1b/manim并在该目录中执行:

```sh
# Install python requirements
python -m pip install -r requirements.txt

# Try it out
python ./manim.py example_scenes.py SquareToCircle -pl
```

将会生成示例并播放。

### 直接使用 (Windows)

1. [安装ffmpeg](https://www.wikihow.com/Install-FFmpeg-on-Windows)。

2. [安装Cairo](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pycairo)。更好的方式是使用 ``pycairo‑1.20.0‑cp38‑cp38m‑win32.whl`` 安装（不会出错）。

   ```sh
   pip install C:\path\to\wheel\pycairo‑1.20.0‑cp38‑cp38m‑win32.whl
   ```

3. 安装 LaTeX 环境。

4. [安装SoX](https://sourceforge.net/projects/sox/files/sox/)。

5. 安装对应的python三方包：

```sh
git clone https://github.com/3b1b/manim.git
cd manim
pip install -r requirements.txt
python manim.py example_scenes.py SquareToCircle -pl
```

### 生成：

```bash
manim ./sum_of_squares -a -p -ql
```

```
-p表示生成后即使用默认播放器播放。

-ql表示低质量生成，速度较快。

-a表示生成代码中所有的类项。
```

### 其他处理

#### 合并音频和视频

（此时需要**将mp3文件放在前面，MP4文件放在后面**）否则会没有背景音乐

```bash
ffmpeg -i .\music\Tassel.mp3 -i .\media\videos\sum_of_squares\480p15\SumOfSquares.mp4 -t 1:45 -y videos\SumOfSquares.mp4
```

-t 后面跟时长 

-y 表示覆盖 

### 调整音频音量

衡量一个音频音量的常用单位是分贝（db），对音频音量处理相关的操作都是基于分贝而来。

#### 查看音频分贝

```bash
ffmpeg -i .\music\Tassel.mp3 -filter_complex volumedetect -c:v copy -f null asd.mp4

[Parsed_volumedetect_0 @ 000002c7f1652540] n_samples: 23821150
[Parsed_volumedetect_0 @ 000002c7f1652540] mean_volume: -13.5 dB
[Parsed_volumedetect_0 @ 000002c7f1652540] max_volume: 0.0 dB
[Parsed_volumedetect_0 @ 000002c7f1652540] histogram_0db: 16907
[Parsed_volumedetect_0 @ 000002c7f1652540] histogram_1db: 36407
```

可以看到最高为0.0db，平均为-13.5db。

#### 1、基于当前音量倍数处理

命令如下，是以当前音量的0.5倍变更输出一个音频，即音量降低了一半；

```bash
ffmpeg  -i .\music\Tassel.mp3 -filter："volume=0.5" output.mp3
```

下面这个是将当前音量提升一倍的处理，这种处理相对粗暴，在表现形式上也是直接对音频波的高度进行的处理，可能会使音频出现失真的现象。

```bash
ffmpeg  -i input.mp3 -filter："volume=2" output.mp3 
```

#### 2、基于分贝数值的处理

上面的基于倍数的处理可能会导致音频的失真，这个基于分贝数值的处理则相对会保留音频的原声效果，如下：

音量提升5分贝（db）；

```bash
ffmpeg  -i input.mp3 -filter：“volume=5dB” output.mp3 
```

音量降低5分贝（db）。

```bash
ffmpeg  -i input.mp3 -filter：“volume=-5dB” output.mp3 
```

#### 3、音量的标准化

除了上面对音量的整体处理，如降低分贝值或者成倍增加音量，ffmpeg还有对音量标准化的处理功能，即削峰填谷，使整个音频的音量变化跨度降低，变得平滑，可能会听起来更舒服点吧。

```bash
ffmepg -i input.mp3 -filter:a "loudnorm" output.mp3 

```

#### 4、其他

```bash
ffmpeg -i <input> -af "volume=xxxdB" <output> 

```

将音量设置到50％：

```bash
ffmpeg -i <input> -af "volume=0.5" <output> 

```

```bash
ffmpeg -i <input> -af "volume=xxxdB,volume=0.5" <output>

```

## 一些视频链接如下：

#### 近期视频链接：

>* [自然数平方和公式证明](https://www.bilibili.com/video/BV1DX4y1P7df)
>* [自然数和公式证明](https://www.bilibili.com/video/BV1ZK4y1W7vw)

## 关于manim

以下是一些中文教程和文档，希望能对你的manim学习有所帮助：

https://manim.ml/

### 中文安装指南

- [一视数学卷毛杨的专栏教程](https://www.bilibili.com/read/cv4139851)
- [一视数学卷毛杨的互动版视频教程](https://www.bilibili.com/video/BV1ap4y1C7NF)
- [pdcxs的专栏教程](https://www.bilibili.com/read/cv3387999)
- [pdcxs关于vscode自动运行脚本的教程](https://www.bilibili.com/read/cv4152112)

### 中文教程

- [mk制作的视频教程(更新中)](https://space.bilibili.com/171431343/favlist?fid=947158443)
- [cigar666的专栏教程文集](https://www.bilibili.com/read/readlist/rl82339)
- [pdcxs转载自Elteoremadebeethoven的英文教程](https://www.bilibili.com/video/av64023740)(因为YouTube在中国大陆无法访问)
- [cai-hust的中文教程](https://github.com/cai-hust/manim-tutorial-CN)

在使用中针对你可能会出现的常见问题，这里有一个文档：[manim常见问题v2.2](https://github.com/manim-kindergarten/manim_sandbox/blob/master/documents/manim%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98v2.2.pdf)，如果无法在线打开预览，[点此下载](https://github.com/manim-kindergarten/manim_sandbox/blob/master/documents/manim%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98v2.2.pdf?raw=True)

### 中文文档

- [Tridu整理、翻译自英文文档的中文文档](https://manim-kindergarten.github.io/manim_document_zh/)
- [manim_sandbox中的wiki（未完成，仅一部分）](https://github.com/manim-kindergarten/manim_sandbox/wiki)
- [manim常见问题v2.2](https://github.com/manim-kindergarten/manim_sandbox/blob/master/documents/manim%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98v2.2.pdf)

## 关于代码使用的相关声明

    1. 代码主要用作个人学习练手使用，欢迎进行修改和补充。
    2. 允许使用部分相关代码进行视频创作，但如果使用代码较多请注明下出处

相关链接：
----

* [manim-kindergarten中整理的一些代码](https://github.com/manim-kindergarten/manim_sandbox 'manim sandbox')<br>
* [鹤翔万里的b站主页](https://space.bilibili.com/171431343)<br>
* [鹤翔万里的github中的manim项目](https://github.com/Tony031218/manim_projects)<br>
* [有一种悲伤叫颓废的github](https://github.com/136108Haumea)<br>
* [pdcxs的github中的manim项目](https://github.com/pdcxs/ManimProjects)
* [manim中文教程文档](https://manim.ml/)
* [cai-hust整理的manim中文文档](https://github.com/cai-hust/manim-tutorial-CN)<br>