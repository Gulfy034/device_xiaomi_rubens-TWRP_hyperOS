### 红米K50的twrp 设备树 (rubens)

[EN](README.md) / CN

=========================================

红米K50(设备代号 _"rubens"_) 是由小米公司出品的一部面向中高端的智能手机。

> [!TIP]
> 这个twrp仓库分支仅仅为搭载 HyperOS 国行版 的红米K50 (rubens) 而建造。

> 再次声明：只针对 **国行** **稳定版** 的 **HyperOS** 搭建twrp镜像，不要给MIUI刷！！！

## 设备参数

基本信息   | 参数
-------:|:-------------------------
CPU核心     | Octa-core CPU with 4x Arm Cortex-A78 up to 2.85GHz
CPU芯片集 | 联发科天玑 8100
GPU     | Mali-G610 MC6
内存  | 8/12 GB RAM (LPDDR5 6400Mbps)
出厂搭载Android版本 | 12
存储 | 128/256/512 GB (UFS 3.1)
电池 | Li-Po 5500 毫安, 不可拆卸
屏幕 | 1440 x 3200 像素, 6.67 英寸, 60/120 赫兹刷新率

![Redmi K50](https://cdn.cnbj0.fds.api.mi-img.com/b2c-shopapi-pms/pms_1653381863.47942179.png)

## 备注

> 还在修复 vendor分区没办法挂载的问题

> 还在修复 用户数据userdata没法解密的问题

## 功能

测试后可以正常工作的功能:

- [X] ADB 调试功能
- [ ] 解密 (正在处理: userdata FBE 加密)
- [X] 正常显示界面
- [X] Fasbootd 模式
- [X] 刷写分区功能
- [X] MTP传输功能
- [X] Sideload 旁加载功能
- [X] USB OTG 挂载功能
- [X] 触摸震动反馈功能

## 编译

首先创建一个文件夹，名字自选，然后 `cd` 这个文件夹。

首先在 [minimal twrp with aosp tree](https://github.com/minimal-manifest-twrp) 下载足以适合twrp编译的最少的安卓aosp源码:

```
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1
repo sync -j$(nproc --all)     //你也可以根据镜像源的服务器负载尽量减少fetch线程数
```

如果发现repo sync过于缓慢就换清华源，默认的是谷歌的aosp源（最稳定）。

如果你在清华源或者其他的镜像源中sync中有错误（例如“不是一个git仓库”错误），那么大概率就是那边的镜像源正在同步谷歌的官方源，所以导致有些资源会出现fetch错误。

有些CN镜像源要求你关闭某些上网工具，不然会导致请求数/重定向次数 过多。

----------

同步好之后在 .repo/manifest.xml 中添加这个:

（给noob提示一下：在manifest标签里面添加）

```xml
<project path="device/xiaomi/rubens" name="D8100-9000-TWRP-Device-Tree/device_xiaomi_rubens-TWRP" remote="github" revision="twrp-13" />
```

再开始编译之前记得把这整个仓库重命名为rubens，然后放在 <当前repo>/device/xaiomi/`<rubens>`

最后开始编译：

```
source build/envsetup.sh
repopick <needed patch>   //你可以选择执行，不过你需要patch一个已知的repopick编号
lunch twrp_rubens-eng    //或者直接lunch，然后再输入有 twrp_rubens-eng 的序号
mka vendorbootimage -j$(nproc --all)
```
## 怎么找到编译好的镜像然后刷呢？

在编译完成会输出一串字符，会告诉你编译完成后输出img镜像的地址，于是你就找到后刷就好了

> [!WARNING]
> 注意因为是联发科的芯片，你需要把twrp刷在 vendor_boot_a 分区里。
> 不然一旦刷错分区只能自求多福了。

```
fastboot flash vendor_boot_a out/target/product/rubens/vendor_boot.img
```
