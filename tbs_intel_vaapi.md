1. 关于为什么不用intel qsv,而选择vaapi?

    答： (1) intel qsv 主要是针对服务器芯片的，对桌面的芯片的支持比较少，主要集中在酷睿4代和5代cpu,而老版本的cpu编解码支持比较少。

    如下图 intel 支持的编解码：

| Platform Name | Graphics | Adds support for...                             |
| ------------- | -------- | ----------------------------------------------- |
| Ironlake      | gen5     | MPEG-2, H.264 decode.                           |
| Sandy Bridge  | gen6     | VC-1 decode; H.264 encode.                      |
| Ivy Bridge    | gen7     | JPEG decode; MPEG-2 encode.                     |
| Bay Trail     | gen7     | -                                               |
| Haswell       | gen7.5   | -                                               |
| Broadwell     | gen8     | VP8 decode.                                     |
| Braswell      | gen8     | H.265 decode; JPEG, VP8 encode.                 |
| Skylake       | gen9     | H.265 encode.                                   |
| Apollo Lake   | gen9     | VP9, H.265 Main10 decode.                       |
| Kaby Lake     | gen9.5   | VP9 profile 2 decode; VP9, H.265 Main10 encode. |
| Coffee Lake   | gen9.5   | -                                               |
| Gemini Lake   | gen9.5   | -                                               |
| Cannonlake    | gen10    | -                                               |

(Each new platform supports a superset of the capabilities of the previous platform.)

​	(2)  intel qsv 在Linux 下专业版收费，libva 开源，自由。因此在linux intel 媒体硬件编解码方案上，著名的ffmpeg 对libva的支持要多于qsv. 

Intel® Media Server Studio 2018 for Linux* ：

|                                                              | **Community Edition**                                        | **Essentials Edition**                                       | **Professional Edition**                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **FREE 30-DAY EVALUATION**[End User License Agreement](https://software.intel.com/en-us/articles/intel-media-server-studio-eula) | [Download](https://registrationcenter.intel.com/RegCenter/comform.aspx?productid=2411) | No trial needed.                                             | [Download](https://registrationcenter.intel.com/en/forms/?productid=3066) |
| **BUY NOW**                                                  | **FREE Download**                                            | **From $499**[Buy Now](https://softwarestore.intel.com/SuiteSelection/MediaServerStudio)[Find a Reseller](https://software.intel.com/en-us/intel-software-products-resellers/) | **From $3999**[Buy Now](https://softwarestore.intel.com/SuiteSelection/MediaServerStudio)[Find a Reseller](https://software.intel.com/en-us/intel-software-products-resellers/) |
| Intel® Media SDK Hardware-accelerated HEVC, AVC, MPEG-2, and more |                                                              |                                                              |                                                              |
| Graphics Drivers                                             |                                                              |                                                              |                                                              |
| Code Samples                                                 |                                                              |                                                              |                                                              |
| [Intel® SDK for OpenCL™ Applications](https://software.intel.com/en-us/opencl-code-builder) |                                                              |                                                              |                                                              |
| Metrics Monitor Linux* only                                  |                                                              |                                                              |                                                              |
| Priority Support Includes confidential, direct access to Intel technical experts. |                                                              |                                                              |                                                              |
| HEVC Codec Components Software and CPU-accelerated decoder and encoder |                                                              |                                                              |                                                              |
| Video Quality Caliper                                        |                                                              |                                                              |                                                              |
| [Intel® VTune™ Amplifier](https://software.intel.com/en-us/intel-vtune-amplifier-xe) |                                                              |                                                              |                                                              |
| **LICENSES**                                                 | **Community Edition**                                        | **Essentials Edition**                                       | **Professional Edition**                                     |
|                                                              | **Named User License:** N/A**Renewal:** N/A                  | **Named User License**$499**Renewal**: A range of prices. [Contact Reseller†](https://software.intel.com/en-us/intel-software-products-resellers/) | $3999**Renewal**: A range of prices. [Contact Reseller†](https://software.intel.com/en-us/intel-software-products-resellers/) |

2. 关于 libva 介绍。

	VAAPI（视频加速API）是一种开源库和API规范，可以访问图形硬件加速功能以进行视频处理。它由一个主库和每个支持的硬件供应商的特定于驱动程序的加速后端组成。

	VAAPI for Intel显卡目前支持以下功能：

	硬件支持

	- 英特尔®GMAX4500HD。
	- 英特尔®高清显卡（采用英特尔®2010Core™i7 / i5 / i3处理器系列）。
	- 英特尔®HDGraphics 2000/3000（采用第二代Intel®Core™i7 / i5 / i3处理器系列）。
	- 英特尔®HDGraphics 2500/4000（采用第3代Intel®Core™i7 / i5 / i3处理器系列）。
	- 英特尔®高清显卡4200/4400/4600/5000，英特尔®Iris™图形5100和英特尔®Iris™Pro图形5200（采用第4代英特尔®酷睿™i7 / i5 / i3处理器系列）。

	特征

	- 支持MPEG2解码加速Main Profile @ HL高达80 Mbps。
	- 支持MPEG2编码加速主要配置文件@ HL高达80 Mbps（不包括GMA X4500HD，HD Graphics和HD Graphics 2000/3000）
	- 支持VC-1解码加速高级配置文件@ Level 3高达40 Mbps（不包括GMA X4500HD和HD Graphics）
	- 支持H.264 / AVC解码加速High Profile @ L4.1高达40 Mbps，具有：
		- H.264 / AVC硬件可变长度解码（VLD） - CABAC
		- H.264 / AVC硬件可变长度解码（VLD） - CAVLC
		- H.264 / AVC硬件逆变换（IT）
		- H.264 / AVC硬件运动补偿（HWMC）
		- H.264 / AVC硬件环路解块（ILDB）
	- 支持H.264 / AVC编码加速Main Profile @ L4.1高达40 Mbps（不包括GMA X4500HD和HD Graphics）。
	- 支持JPEG / MJPEG解码（不包括GMA X4500HD，HD Graphics和HD Graphics 2000/3000）。
	- 支持视频后期处理（不包括GMA X4500HD）。

	| VAAPI能力                   | MPEG2解码 | MPEG2编码  | H.264 / AVC解码  | VC-1解码 | H.264 / AVC编码 | JPEG / MJPEG | 视频后期处理 |
	| --------------------------- | --------- | ---------- | ---------------- | -------- | --------------- | ------------ | ------------ |
	| 英特尔®GMAX4500HD           | 是        | 硬件不支持 | 是的g45-h264分支 | 未实现   | 硬件不支持      | 硬件不支持   | 没有         |
	| Intel®HDGraphics            | 是        | 硬件不支持 | 是               | 未实现   | 硬件不支持      | 硬件不支持   | 是           |
	| 英特尔®HDGraphics 2000/3000 | 是        | 硬件不支持 | 是               | 是       | 是              | 硬件不支持   | 是           |
	| 英特尔®IvyBridge            | 是        | 是         | 是               | 是       | 是              | 是           | 是           |
	| 英特尔®Haswell              | 是        | 是         | 是               | 是       | 是              | 是           | 是           |

	要了解有关VA API开发的更多信息并参与该过程，以下是入门的主要指导原则：

	Github资源

	- Libva项目位于<https://github.com/intel/libva>
	- 英特尔驱动程序项目位于<https://github.com/intel/intel-vaapi-driver>
	- Libva utils项目已移至<https://github.com/intel/libva-utils>

	问题

	- 使用<https://github.com/intel/libva/issues/new>在libva github网站上提交bug
	- 使用<https://github.com/intel/intel-vaapi-driver/issues/new>在intel-vaapi-driver github网站上提交bug
	- 使用<https://github.com/intel/libva-utils/issues/new>在libva-utils github网站上提交bug

	邮件列表

	- 一般开发人员的讨论发生在[intel-vaapi-media@lists.01.org](mailto:intel-vaapi-media@lists.01.org)  邮件列表中。
	- 订阅<https://lists.01.org/mailman/listinfo/intel-vaapi-media>上的列表
	- 档案可在<http://lists.01.org/pipermail/intel-vaapi-media/>找到 

	IRC

	- 英特尔的VA API开发人员在  [## ](irc://freenode.net/#intel-gfx)[intel-media@freenode.net上闲逛](mailto:media@freenode.net)。

3.  ffmpeg支持英特尔GPU上的视频硬件加速 流程框图

	![img](https://01.org/sites/default/files/users/u24924/ffmpeg.png)

	[FFmpeg-vaapi](http://trac.ffmpeg.org/wiki/Hardware/VAAPI)是一个FFmpeg插件，它提供基于低级VAAPI接口的硬件加速，利用行业标准VA API在英特尔GPU上执行高性能视频编解码器，视频处理和转码功能。

	[FFmpeg-qsv](http://trac.ffmpeg.org/wiki/Hardware/QuickSync)是一个FFmpeg插件，提供基于Intel GPU的硬件加速。它提供基于Intel [Media SDK](https://github.com/Intel-Media-SDK/MediaSDK)库的高性能视频编解码器，视频处理和转码功能。

	FFmpeg-ocl是一个FFmpeg插件，它在CPU / GPU 上提供基于工业标准OpenCL的硬件加速。它主要用于加速视频处理过滤器。

	VAAPI（视频加速API）是一个开源库（[LibVA](https://github.com/intel/libva)）和API规范，可以访问视频编解码器和处理的图形硬件加速功能。[Libva-utils](https://github.com/intel/libva-utils)是[VAAPI](https://github.com/intel/libva-utils)的测试和示例的集合。

	VAAPI驱动程序是基于LibVA的硬件加速视频驱动程序。它是用户模式驱动程序（UMD）。英特尔为英特尔GPU提供两个开源VAAPI驱动程序：[intel-vaapi-driver](https://github.com/intel/intel-vaapi-driver) （旧版）和[intel-media-driver](https://github.com/intel/media-driver/) （新版）。

	

	4.英特尔媒体堆栈

	英特尔媒体堆栈是一套全面的软件包，用于Linux *上的硬件加速视频编码，解码和处理。英特尔图形处理单元（GPU）包含一个或多个基于硬件的解码器，编码器和视频处理器，用于几种流行的编解码器和功能。 

	[![img](https://01.org/sites/default/files/resize/users/u52675/image2018-7-27_11_28_56-403x300.png)](https://01.org/sites/default/files/users/u52675/image2018-7-27_11_28_56.png)

	*该图表示支持英特尔下一代处理器，代号为Ice Lake。

	 

	通过利用FFmpeg *，Gstreamer *，Open Broadcaster Software *，Intel®MediaSDK和Chrome OS *媒体框架中提供的集成，您可以使用现有应用程序在GPU上解码，编码和处理硬件加速器。

	基于强大的硬件支持，一些客户选择英特尔®  图形  技术  用于流行用途，例如：  游戏流，云游戏，视频分析，短视频，视频云，车载信息娱乐系统（IVI），视频监控等。

	英特尔®  媒体堆栈 提供基于英特尔®图形技术的全媒体解决方案  。它包括 英特尔®  媒体驱动程序， 英特尔®  媒体SDK，  LibVA，中间件支持和HDCP服务。英特尔®媒体堆栈还支持着名的开源媒体框架，如FFmpeg和Gstreamer。 

	下图显示了所有媒体堆栈组件。

	[![img](https://01.org/sites/default/files/resize/users/u52675/image2018-8-15_11_17_14-624x300.png)](https://01.org/sites/default/files/users/u52675/image2018-8-15_11_17_14.png)

	- 媒体驱动程序： 

		开源硬件加速视频驱动程序，从Broadwell开始支持英特尔®高清显卡。以下是当前支持的处理器列表：

		- 采用英特尔®高清显卡的第9代Intel®Core™处理器 - Ice Lake
		- 采用Intel®HDGraphics的第8代Intel®Core™处理器 -  [Coffee Lake](https://ark.intel.com/products/codename/97787/Coffee-Lake#@Coffee-Lake)
		- 采用Intel®HDGraphics的第四代Intel®Pentium/ Celeron™处理器 -  [Gemini-Lake](https://ark.intel.com/products/codename/83915/Gemini-Lake?q=Gemini%20Lake)
		- 采用英特尔®高清显卡的第7代Intel®Core™处理器 -  [Kaby Lake](http://ark.intel.com/products/codename/82879/Kaby-Lake#@All)
		- 采用Intel®HDGraphics的第三代Intel®Pentium/ Celeron / Atom™处理器 -  [Apollo-Lake](https://ark.intel.com/products/codename/80644/Apollo-Lake?q=Apollo%20lake)
		- 采用英特尔®高清显卡的第六代英特尔®酷睿™处理器 -  [Skylake](http://ark.intel.com/products/codename/37572/Skylake#@All)
		- 采用[Intel®HD](http://ark.intel.com/products/codename/38530/Broadwell#@All)显卡的第五代Intel®Core™处理器 -  [Broadwell](http://ark.intel.com/products/codename/38530/Broadwell#@All)

	- 用于访问硬件功能的源代码开发库和SDK：

		- LibVA：  对于VA-API（视频加速API）的实现-一个开放源码库，提供了访问图形硬件加速功能。
		- LibVA-utils：  根据libva项目运行VA-API的一组实用程序和示例。
		- Media SDK：   用于在具有集成图形的Intel平台上访问硬件加速视频解码，编码和过滤的API。 见d上etails [媒体SDK页面](http://mediasdk.intel.com/)。
		- 360 SDK：   在开源计划中提供带有2/6摄像头输入的360拼接。
		- Capture SDK：   在开源计划中为游戏流提供游戏捕获解决方案。

	- 流行用法的参考设计： 

		- 视频分析：  基于openVINO和clDNN，在开源计划。

	- 可访问硬件功能的第三方框架：

		- FFMPEG：  的集成支持 经由avcodec中英特尔图形硬件加速视频解码，编码和处理。有两种可能的路径：一种用于VAAPI，另一种用于QSV，代表Media SDK。有关 详细信息，请参阅  [ffmpeg-vaapi](https://trac.ffmpeg.org/wiki/Hardware/VAAPI)  和  [ffmpeg-qsv](https://trac.ffmpeg.org/wiki/Hardware/QuickSync)。
		- gstreamer：  支持英特尔图形上的加速视频解码，编码和处理。有两种可能的路径：一种用于VAAPI，另一种用于Media SDK。有关 详细信息，请参阅 [gstreamer-vaapi](https://cgit.freedesktop.org/gstreamer/gstreamer-vaapi/) 和 [gstreamer-MSDK](https://cgit.freedesktop.org/gstreamer/gst-plugins-bad/tree/sys/msdk)。

	- 内容保护：

		- HDCP：   我 ncludes HDCP SDK和HDCP SRV提供基于图形GEN HDCP解决方案。

	## 源代码

	### 英特尔自有组件

	| 零件                                        | 地点                                               |
	| ------------------------------------------- | -------------------------------------------------- |
	| 英特尔®  媒体驱动程序（参见注释）           | <https://github.com/intel/media-driver>            |
	| 英特尔®  媒体SDK                            | <https://github.com/Intel-Media-SDK/MediaSDK>      |
	| 英特尔HDCP                                  | <https://github.com/intel/hdcp>                    |
	| 英特尔Libva                                 | <https://github.com/intel/libva>                   |
	| 英特尔Libva示例代码                         | <https://github.com/intel/libva-utils>             |
	| 英特尔®C  -for-Media编译器                  | <https://github.com/intel/cm-compiler>             |
	| 英特尔®  图形内存管理库                     | <https://github.com/intel/gmmlib>                  |
	| 适用于Android * OS的英特尔硬件编辑器        | <https://github.com/INTEL/HWC>                     |
	| 3D图形库                                    | <https://github.com/mesa3d/mesa>                   |
	| 深度神经网络计算库                          | <https://github.com/intel/clDNN>                   |
	| 用于OpenCL™驱动程序的英特尔® 图形计算运行时 | <https://github.com/intel/compute-runtime>         |
	| 适用于OpenCL™的英特尔® 图形编译器           | <https://github.com/intel/intel-graphics-compiler> |

	注意：vaapi驱动程序最多支持Cannon Lake，它将被Intel®Media驱动程序取代。

	 

	### 第三方组件

	| 零件                 | 地点                                                         |
	| -------------------- | ------------------------------------------------------------ |
	| DRM                  | <https://anongit.freedesktop.org/git/mesa/drm.git>           |
	| ffmpeg的             | <https://git.ffmpeg.org/ffmpeg.git>                          |
	| GStreamer的-VAAPI    | [https://anongit.freedesktop.org/git/gstreamer/gstreamer-vaapi.git](https://anongit.freedesktop.org/git/gstreamer/gstreamer-vaapi.git/) |
	| GStreamer的-MediaSDK | [https://anongit.freedesktop.org/git/gstreamer/gst-plugins-bad.git](https://anongit.freedesktop.org/git/gstreamer/gst-plugins-bad.git/) |

5. 搭建centos7 下， ffmpeg +x264 +x265+ libva（intel的环境。

	（目前推荐使用旧版本的libva驱动—— [intel-vaapi-driver](https://github.com/intel/intel-vaapi-driver) ，主要原因支持的比较多，兼容以前的设备.如果想装新版本驱动[intel-media-driver](https://github.com/intel/media-driver/) ，点击超链接见维基，初次之外其他步骤一样） 

	检查自己的centos7 系统下 是否有 /dev/dri/renderD128  ，没有的话要升级支持的intel显卡的linux内核。

	依赖库整理： 
	libffi-3.0.13-11.el7.x86_64.rpm 
	libffi-devel-3.0.13-11.el7.x86_64.rpm 
	libdrm-2.4.56-2.el7.x86_64.rpm 
	libdrm-devel-2.4.56-2.el7.x86_64.rpm

	expat-2.1.0-8.el7.x86_64.rpm 
	expat-devel-2.1.0-8.el7.x86_64.rpm 
	yasm-1.2.0-4.el7.x86_64.rpm 
	lynx-2.8.8-0.3.dev15.el7.x86_64.rpm

	以下几个需要依赖库比较多，建议通过仓库直接装，或根据yum提示自行准备库： 
	yum install xmlto 
	yum install graphviz 
	yum install cmake 
	yum install automake libtool

	因为我们还要直接调用libva的X11接口，所以 
	yum install xorg-x11*

	准备wayland： 
	yum install libpciaccess-devel 
	git://anongit.freedesktop.org/wayland/wayland

	开始编译：

	export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH

	#首先编译wayland
	cd wayland
	./autogen.sh
	make
	make install

	#编译libva和intel驱动
	cd libva-1.6.0
	autoreconf
	./configure

	make
	make install

	

	cd libva-intel-driver-1.6.0
	autoreconf
	./configure

	make
	make install

	#编译x264
	cd x264
	./configure --enable-pic --enable-shared
	make
	make install

	#编译x265
	cd x265
	cd build/linux/
	./make-Makefiles.bash
	make
	make install

	#最后编译FFMpeg

	export LD_LIBRARY_PATH="/usr/local/lib:/usr/lib:/lib"
	./configure   --disable-shared --enable-static --enable-gpl --enable-nonfree --enable-version3  --enable-asm  --enable-libx264 --enable-pthreads  --prefix=/usr/local   --extra-libs=-ldl  --extra-cflags="-I/usr/local/include -DREDIRECT_DEBUG_LOG" --extra-ldflags="-L/usr/local/lib -gl -lva -lva-drm -lva-x11"    --enable-libx265 
	make

	make install

	测试：

	Decode an input and discard the output (this can be used as a crude benchmark of the decoder):

	```
	ffmpeg -hwaccel vaapi -hwaccel_device /dev/dri/renderD128 -hwaccel_output_format vaapi -i input.mp4 -f null -
	```

	Encode an input with H.264 at 5Mbps VBR:

	```
	ffmpeg -vaapi_device /dev/dri/renderD128 -i input.mp4 -vf 'format=nv12,hwupload' -c:v h264_vaapi -b:v 5M output.mp4
	```

	Hardware-only transcode to H.264 at 2Mbps CBR:

	```
	ffmpeg -hwaccel vaapi -hwaccel_device /dev/dri/renderD128 -hwaccel_output_format vaapi -i input.mp4 -c:v h264_vaapi -b:v 2M -maxrate 2M output.mp4
	```

	Capture a raw stream from a V4L2 camera device and encode it as H.264:

	```
	ffmpeg -vaapi_device /dev/dri/renderD128 -f v4l2 -video_size 1920x1080 -i /dev/video0 -vf 'format=nv12,hwupload' -c:v h264_vaapi output.mp4
	```

6. 关于libva 在ffmpeg的一些参数

	-vaapi_device device  set VAAPI hardware device (DRM path or X11 display name)

	h264_vaapi AVOptions:
	  -low_power         <boolean>    E..V..... Use low-power encoding mode (only available on some platforms; may not support all encoding features) (default false)
	  -qp                <int>        E..V..... Constant QP (for P-frames; scaled by qfactor/qoffset for I/B) (from 0 to 52) (default 20)
	  -quality           <int>        E..V..... Set encode quality (trades off against speed, higher is faster) (from -1 to INT_MAX) (default -1)
	  -coder             <int>        E..V..... Entropy coder type (from 0 to 1) (default cabac)
	     cavlc                        E..V.....
	     cabac                        E..V.....
	     vlc                          E..V.....
	     ac                           E..V.....
	  -aud               <boolean>    E..V..... Include AUD (default false)
	  -sei               <flags>      E..V..... Set SEI to include (default identifier+timing+recovery_point)
	     identifier                   E..V..... Include encoder version identifier
	     timing                       E..V..... Include timing parameters (buffering_period and pic_timing)
	     recovery_point               E..V..... Include recovery points where appropriate
	  -profile           <int>        E..V..... Set profile (profile_idc and constraint_set*_flag) (from -99 to 65535) (default -99)
	     constrained_baseline              E..V.....
	     main                         E..V.....
	     high                         E..V.....
	  -level             <int>        E..V..... Set level (level_idc) (from -99 to 255) (default -99)
	     1                            E..V.....
	     1.1                          E..V.....
	     1.2                          E..V.....
	     1.3                          E..V.....
	     2                            E..V.....
	     2.1                          E..V.....
	     2.2                          E..V.....
	     3                            E..V.....
	     3.1                          E..V.....
	     3.2                          E..V.....
	     4                            E..V.....
	     4.1                          E..V.....
	     4.2                          E..V.....
	     5                            E..V.....
	     5.1                          E..V.....
	     5.2                          E..V.....
	     6                            E..V.....
	     6.1                          E..V.....
	     6.2                          E..V.....

	

	h265_vaapi AVOptions:
	  -low_power         <boolean>    E..V..... Use low-power encoding mode (only available on some platforms; may not support all encoding features) (default false)
	  -qp                <int>        E..V..... Constant QP (for P-frames; scaled by qfactor/qoffset for I/B) (from 0 to 52) (default 25)
	  -aud               <boolean>    E..V..... Include AUD (default false)
	  -profile           <int>        E..V..... Set profile (general_profile_idc) (from -99 to 255) (default -99)
	     main                         E..V.....
	     main10                       E..V.....
	     rext                         E..V.....
	  -tier              <int>        E..V..... Set tier (general_tier_flag) (from 0 to 1) (default main)
	     main                         E..V.....
	     high                         E..V.....
	  -level             <int>        E..V..... Set level (general_level_idc) (from -99 to 255) (default -99)
	     1                            E..V.....
	     2                            E..V.....
	     2.1                          E..V.....
	     3                            E..V.....
	     3.1                          E..V.....
	     4                            E..V.....
	     4.1                          E..V.....
	     5                            E..V.....
	     5.1                          E..V.....
	     5.2                          E..V.....
	     6                            E..V.....
	     6.1                          E..V.....
	     6.2                          E..V.....
	  -sei               <flags>      E..V..... Set SEI to include (default hdr)
	     hdr                          E..V..... Include HDR metadata for mastering display colour volume and content light level information

	

	mjpeg_vaapi AVOptions:
	  -low_power         <boolean>    E..V..... Use low-power encoding mode (only available on some platforms; may not support all encoding features) (default false)
	  -jfif              <boolean>    E..V..... Include JFIF header (default false)
	  -huffman           <boolean>    E..V..... Include huffman tables (default true)

	mpeg2_vaapi AVOptions:
	  -low_power         <boolean>    E..V..... Use low-power encoding mode (only available on some platforms; may not support all encoding features) (default false)
	  -profile           <int>        E..V..... Set profile (in profile_and_level_indication) (from -99 to 7) (default -99)
	     simple                       E..V.....
	     main                         E..V.....
	  -level             <int>        E..V..... Set level (in profile_and_level_indication) (from 0 to 15) (default high)
	     low                          E..V.....
	     main                         E..V.....
	     high_1440                    E..V.....
	     high                         E..V.....

	

	vp8_vaapi AVOptions:
	  -low_power         <boolean>    E..V..... Use low-power encoding mode (only available on some platforms; may not support all encoding features) (default false)
	  -loop_filter_level <int>        E..V..... Loop filter level (from 0 to 63) (default 16)
	  -loop_filter_sharpness <int>        E..V..... Loop filter sharpness (from 0 to 15) (default 4)

	

	vp9_vaapi AVOptions:
	  -low_power         <boolean>    E..V..... Use low-power encoding mode (only available on some platforms; may not support all encoding features) (default false)
	  -loop_filter_level <int>        E..V..... Loop filter level (from 0 to 63) (default 16)
	  -loop_filter_sharpness <int>        E..V..... Loop filter sharpness (from 0 to 15) (default 4)

	

	deinterlace_vaapi AVOptions:
	  mode              <int>        ..FV..... Deinterlacing mode (from 0 to 4) (default default)
	     default                      ..FV..... Use the highest-numbered (and therefore possibly most advanced) deinterlacing algorithm
	     bob                          ..FV..... Use the bob deinterlacing algorithm
	     weave                        ..FV..... Use the weave deinterlacing algorithm
	     motion_adaptive              ..FV..... Use the motion adaptive deinterlacing algorithm
	     motion_compensated              ..FV..... Use the motion compensated deinterlacing algorithm
	  rate              <int>        ..FV..... Generate output at frame rate or field rate (from 1 to 2) (default frame)
	     frame                        ..FV..... Output at frame rate (one frame of output for each field-pair)
	     field                        ..FV..... Output at field rate (one frame of output for each field)
	  auto              <int>        ..FV..... Only deinterlace fields, passing frames through unchanged (from 0 to 1) (default 0)

	

	denoise_vaapi AVOptions:
	  denoise           <int>        ..FV..... denoise level (from 0 to 64) (default 0)

	

	procamp_vaapi AVOptions:
	  b                 <float>      ..FV..... Output video brightness (from -100 to 100) (default 0)
	  brightness        <float>      ..FV..... Output video brightness (from -100 to 100) (default 0)
	  s                 <float>      ..FV..... Output video saturation (from 0 to 10) (default 1)
	  saturatio         <float>      ..FV..... Output video saturation (from 0 to 10) (default 1)
	  c                 <float>      ..FV..... Output video contrast (from 0 to 10) (default 1)
	  contrast          <float>      ..FV..... Output video contrast (from 0 to 10) (default 1)
	  h                 <float>      ..FV..... Output video hue (from -180 to 180) (default 0)
	  hue               <float>      ..FV..... Output video hue (from -180 to 180) (default 0)

	

	scale_vaapi AVOptions:
	  w                 <string>     ..FV..... Output video width (default "iw")
	  h                 <string>     ..FV..... Output video height (default "ih")
	  format            <string>     ..FV..... Output video format (software format of hardware frames)
	  mode              <int>        ..FV..... Scaling mode (from 0 to 768) (default hq)
	     default                      ..FV..... Use the default (depend on the driver) scaling algorithm
	     fast                         ..FV..... Use fast scaling algorithm
	     hq                           ..FV..... Use high quality scaling algorithm
	     nl_anamorphic                ..FV..... Use nolinear anamorphic scaling algorithm

	

	sharpness_vaapi AVOptions:
	  sharpness         <int>        ..FV..... sharpness level (from 0 to 64) (default 44)