---
title: Android Image Format
tags: 图像格式
categories: Android
---



介绍**Android**系统中image format类型和数据格式。
图形相关的image format定义在[android.graphics.ImageFormat](https://developer.android.com/reference/android/graphics/ImageFormat.html).

## RGB
> RGB代表红、绿、蓝三个颜色通道，每个点的颜色都是用这三种颜色的值叠加。
>
> ###RGB_565
>
> >  RGB565使用16位表示一个像素，这16位中的5位用于R，6位用于G，5位用于B。
> >
> >  每一个像素点的数据如下：
> >
> >  R R R R R G G G G G G B B B B B
>
> ### FLEX _RGB _888
>
> > Mutli-plane Android RGB format, plane可以理解为平面，每一个像素点的颜色都由8位的R,G,B颜色决定。
> >
> > 图像数据由三个独立的data buffer表示，每一个buffer代表一个颜色平面。
> >
> > 使用plane方式存储图像数据时，存储数据使用的buffer还会保存**row stride**和**pixel stride**的信息。
> >
> > ```
> > The order of planes in the array returned by Image#getPlanes() is guaranteed such that plane #0 is always R (red), plane #1 is always G (green), and plane #2 is always B (blue).
> > All three planes are guaranteed to have the same row strides and pixel strides.
> > ```
>
> ### FLEX _RGBA _8888
>
> > 比FLEX _RGB _888多了一个alpha通道，
> >
> > ```
> > The order of planes in the array returned by Image#getPlanes() is guaranteed such that plane #0 is always R (red), plane #1 is always G (green), plane #2 is always B (blue), and plane #3 is always A (alpha). This format may represent pre-multiplied or non-premultiplied alpha.
> > All four planes are guaranteed to have the same row strides and pixel strides.
> > ```
>
> ###YUV_420 _888, YUV_422 _888, YUV_444 888
> > 类似于**RGB**，**YUV**也是一种颜色编码格式，其中Y表示明亮度，U和V表示色度，浓度。
> >
> > YUV，YCbCr都称为YUV。
> >
> > YUV的存储格式有两种，一种是紧缩格式（packed format），存储方式类似于RGB，所有的值保存在一个矩阵中，每个像素点的Y,U,V值，紧挨着放在一起。另外一种是平面格式（planar format），将Y,U,V三个分量分别放在不同的矩阵中。
> >
> > 根据每个像素点采样的不同，YUV数据又可以分为YUV420，YUV422，YUV444
> >
> > * YUV444：表示完全采样，每个像素点都有自己的Y分量和UV分量。
> > * YUV422：表示水平2:1采样，垂直完全采样。每个像素点都有自己的Y分量，水平方向，每两个像素点共用UV分量。
> > * YUV420：表示水平和垂直都是2:1采样，每个像素点都有自己的Y分量，水平和垂直方向，每四个点共用UV分量。
>
> ###NV16和NV21
> > **NV16**是YUV422SP，**NV21**是YUV420SP，Y分量是一个平面（plane），UV分量是一个平面（plane）。
> >
> > 后缀**SP**的意思是Semi-Planar,表示图像数据不是分成三个平面，而是两个。
> >
> > ```
> > NV16 is YCbCr format, used for video. For camera, the YUV_420_888 format is recommanded for YUV output instead.
> > NV21 is YCrCb format used for images, which uses the NV21 encoding format. This is the default format for Camera preview images, when not otherwise set with setPreviewFormat(int).
> > ```
>
> ###RAW10和RAW12
> >**RAW**是记录图像传感器未被处理的原始图像数据。
> >
> >- **RAW10**表示每个像素用10bit的数据表示，具体格式如下，第五个byte会记录前面四个像素的第0bit和第1bit。
> >
> >    | bit 7	|bit 6|bit 5|	bit 4|	bit 3|bit 2| bit 1| bit 0
> >     ---| ------|------|------|------|-----|------|-----|--------
> >    Byte 0:|P0[9]|	P0[8]| P0[7]	|P0[6]|P0[5]| P0[4]|P0[3]| P0[2]
> >    Byte 1:|P1[9]|	P1[8]|	P1[7]	|P1[6]|P1[5]| P1[4]| P1[3]|P1[2]
> >    Byte 2:|P2[9]|	P2[8]|	P2[7]	|P2[6]|P2[5]| P2[4]| P2[3]|P2[2]
> >    Byte 3:|P3[9]|	P3[8]|	P3[7]	|P3[6]|P3[5]| P3[4]| P3[3]| P3[2]
> >    Byte 4:|P3[1]|	P3[0]|	P2[1]	|P2[0]|P1[1]| P1[0]| P0[1]| P0[0]

> > - **RAW12**表示每个像素用12bit的数据表示，具体格式如下，第三个byte会记录前面两个像素点的第0～3位。
> >
> >     | bit 7   | bit 6  | bit 5  | bit 4  | bit 3  | bit 2  | bit 1  | bit 0  |
> >     | ------- | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
> >     | Byte 0: | P0[11] | P0[10] | P0[ 9] | P0[ 8] | P0[ 7] | P0[ 6] | P0[ 5] |
> >     | Byte 1: | P1[11] | P1[10] | P1[ 9] | P1[ 8] | P1[ 7] | P1[ 6] | P1[ 5] |
> >     | Byte 2: | P1[ 3] | P1[ 2] | P1[ 1] | P1[ 0] | P0[ 3] | P0[ 2] | P0[ 1] |


> ### RAW _PRIVATE 和 RAW _SENOR
> > - **RAW_PRIVATE**是依赖具体实现的raw格式.
> >
> > ```
> > Private raw camera sensor image format, a single channel image with implementation depedent pixel layout.
> > ```

>> RAW_PRIVATE is a format for unprocessed raw image buffers coming from an image sensor. The actual structure of buffers of this format is implementation-dependent.
>> ```
>>
>> - **RAW_SENSOR**
>>
>> ```
>> General raw camera sensor image format, usually representing a single-channel Bayer-mosaic image. Each pixel color sample is stored with 16 bits of precision.

>> The layout of the color mosaic, the maximum and minimum encoding values of the raw pixel data, the color space of the image, and all other needed information to interpret a raw sensor image must be queried from the CameraDevice which produced the image.
>> ```
>>
>> ```
>
> ###PRIVATE
>
> >私有的，不透明的format，应用无法获得其具体的图像数据，只能通过相关接口做图像处理。
> >
> > ```
> > Android private opaque image format.
> > ```

> > The choices of the actual format and pixel data layout are entirely up to the device-specific and framework internal implementations, and may vary depending on use cases even for the same device. The buffers of this format can be produced by components like ImageWriter , and interpreted correctly by consumers like CameraDevice based on the device/framework private information. However, these buffers are not directly accessible to the application.

>>When an Image of this format is obtained from an ImageReader or ImageWriter, the getPlanes() method will return an empty Plane array.

>>If a buffer of this format is to be used as an OpenGL ES texture, the framework will assume that sampling the texture will always return an alpha value of 1.0 (i.e. the buffer contains only opaque pixel values).
>>```
>>
>>```