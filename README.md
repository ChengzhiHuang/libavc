# libavc

------

## Intruction
<br/>Forked from [AOSP's git](https://android.googlesource.com/platform/external/libavc).
<br/>I add cmake files to it so that it can be built on Linux using X86 CPUs.
<br/>I'm using [CLion](https://www.jetbrains.com/clion/) , so I don't add CMAKE_BUILD_TYPE ,you can add it by yourself , [methods here](https://stackoverflow.com/questions/7724569/debug-vs-release-in-cmake).

------
### Building Instructions
<br/>Went into the working directory first.
```Bash
$ mkdir build
$ cd build
$ cmake ..
$ make
```
<br/>Then the execable file will be generated into  ./bin.

------
### Encode learning scripts 
<br/>先放一段正常的encode的代码，输入是encode一段1080P的yuv文件：`test_14_1920x1080.yuv`，位于bin目录下。输出文件当当前bin目录下，命名为：`avc_test_14.avc`。指定SIMD选项为：`X86_SSE42`，宽为：`1920`，高为：`1080`，需要encode的范围为全部（因为小写的阿拉伯字母赋值到uword32类型是4294967295，你换成a，b，c都可以）：`l`，输入文件的格式为：`YUV_420P`，进行encode的逻辑核心数是：`4`，profile选择：`MAIN`,输出文件的帧率为：`30`，bitrate为：`8000000`，也就是8Mbps。
```Bash
$ cd bin
$ ./avcencode --input ./test_14_1920x1080.yuv --output ./avc_test_14.avc --arch X86_SSE42 --width 1920 --height 1080 --num_frames l --input_chroma_format YUV_420P --num_cores 4 --profile MAIN --tgt_framerate 30 --bitrate 8000000
```
------
### Setting I/P/B frames
<br/>默认情况下，B frames是没有的（`#define DEFAULT_NUM_BFRAMES 0`），应该是只有I、P两种frame的。关于这三种frame的简介点[这里](https://en.wikipedia.org/wiki/Video_compression_picture_types).
<br/>如果不放心的话，可以这样设置：
```Bash
--bframes 0
```
<br/>这里插一句，遇到要求说“只需要I/P两种frame”的情况，并非这样设置:
```Bash
--qp_i --qp_p
```
<br/>qp是[quantization parameter](https://www.vcodex.com/news/h264-quantization-parameter/),跟我们设置有没有B frame没关系。
<br/>上面那个还有一个问题是，基本上libavc的选项都是需要你传值进去的，例如这样：
```Bash
--qp_i 25 --qp_p 25
```
<br/>哪怕是开启psnr功能，都不是直接`--psnr`，而是：
```Bash
--psnr 1
```
<br/>然后是选项`--i_interval`,这个选项默认值是30（`#define DEFAULT_I_INTERVAL 30`），意思是每相隔i_interval个P frame，出现一个I frame。
<br/>节选一段输出：
<br/> >[IDR] PicNum    1 Bytes Generated 229223 TimeTaken(microsec):  71692 AvgTime:  71692 PeakAvgTimeMax:   8961
<br/> >[P] PicNum    2 Bytes Generated  18356 TimeTaken(microsec):  40073 AvgTime:  55882 PeakAvgTimeMax:  13970
<br/> >[P] PicNum    3 Bytes Generated  42063 TimeTaken(microsec):  47981 AvgTime:  53248 PeakAvgTimeMax:  19968
<br/> >...
<br/> >[P] PicNum   30 Bytes Generated  39518 TimeTaken(microsec):  74971 AvgTime:  63501 PeakAvgTimeMax:  81982
<br/> >[I] PicNum   31 Bytes Generated 240327 TimeTaken(microsec):  85747 AvgTime:  64218 PeakAvgTimeMax:  81982
<br/> >[P] PicNum   32 Bytes Generated  24927 TimeTaken(microsec):  54735 AvgTime:  63922 PeakAvgTimeMax:  81982

<br/>每一行输出的第一个字母就是表示这一帧的类型，[`IDR frame`](http://blog.csdn.net/darennet/article/details/8208376)就是特指第一个I帧。如果我们修改i_interval的数值就会有相应变化，我接下来设置
```Bash
--i_interval 40
```
<br/>节选一段输出：
<br/> >[IDR] PicNum    1 Bytes Generated 229223 TimeTaken(microsec):  57229 AvgTime:  57229 PeakAvgTimeMax:   7153
<br/> >[P] PicNum    2 Bytes Generated  18356 TimeTaken(microsec):  44132 AvgTime:  50680 PeakAvgTimeMax:  12670
<br/> >...
<br/> >[P] PicNum   40 Bytes Generated  45256 TimeTaken(microsec):  63674 AvgTime:  62748 PeakAvgTimeMax:  78452
<br/> >[I] PicNum   41 Bytes Generated 210770 TimeTaken(microsec):  79987 AvgTime:  63169 PeakAvgTimeMax:  78452
<br/> >[P] PicNum   42 Bytes Generated  79391 TimeTaken(microsec):  89158 AvgTime:  63787 PeakAvgTimeMax:  78452

<br/>

------
### PSNR & Speed
<br/> [峰值信噪比(Peak signal-to-noise ratio)](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio)是一种[视频质量(Video quality)](https://en.wikipedia.org/wiki/Video_quality)的表示指标。就是信噪比SNR的变种，信号与系统这门课里有学，结论就是数值越高，视频质量越好。
<br/>然后一般来说，如果encode的时候speed越快，PSNR这个指标就会越低；反之，如果encode慢一点，PSNR就会得到提高。
<br/>(IVE_SLOWEST = 1,IVE_FASTEST = 5)
<br/>设置：
```Bash
--psnr 1 --speed FASTEST
```
<br/>得到的结果是:
```Bash
Avg PSNR Y : 41.88;Avg PSNR U : 46.14;Avg PSNR V : 47.64
```

<br/>设置：
```Bash
--psnr 1 --speed SLOWEST
```
<br/>得到的结果是
```Bash
Avg PSNR Y : 45.35;Avg PSNR U : 48.82;Avg PSNR V : 50.28
```

------
### RC
<br/>`0: Constant Qp, 1: Storage, 2: CBR non low delay, 3: CBR low delay`
<br/>一般来说，编码的时候会使用[constant bitrate (CBR)](https://en.wikipedia.org/wiki/Constant_bitrate) , 或者是与之相反的是[Variable bitrate (VBR)](https://en.wikipedia.org/wiki/Variable_bitrate).然后VBR用专业术语来说就是[CQP](http://slhck.info/video/2017/02/24/crf-guide.html).

<br/>因此如果要使用`--rc 0`,应该要跟`--qp_i`和`--qp_p`一起使用，这个还没验证过。
<br/>如果要使用`--rc 2`，应该要跟`--bitrate`一起使用，这里注意有些其他库的默认单位是kbps，但是这里是bps.

<br/>`Storage`表示什么，我也不清楚。`CBR low delay`这个我用了报错= =。

------
### Entropy
<br/>[熵编码(entropy encoding)](https://en.wikipedia.org/wiki/Entropy_encoding),libavc提供两种，`0: CAVLC or 1: CABAC`

<br/>[适应性变动长度编码法Context-adaptive variable-length coding (CAVLC)](https://en.wikipedia.org/wiki/Context-adaptive_variable-length_coding)
<br/>[前文参考之适应性二元算术编码Context-adaptive binary arithmetic coding (CABAC)](https://en.wikipedia.org/wiki/Context-adaptive_binary_arithmetic_coding)
<br/>CAVLC支持所有的H.264 profiles，CABAC则不支持Baseline以及Extended profiles。






