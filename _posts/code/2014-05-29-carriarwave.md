---
layout: post
title: CarrierWave
description: This gem provides a simple and extremely flexible way to upload files from Ruby applications. It works well with Rack based web applications, such as Ruby on Rails.
date: 2014-04-29
category: code
---

# 	 一些注意

CarrierWave 是Ruby写的一个方便的image upload的Gem包

*This gem provides a simple and extremely flexible way to upload files from Ruby applications. It works well with Rack based web applications, such as Ruby on Rails.*

CarrierWave::MiniMagick是这个包中处理图片的重要的一个[Module][2]

resize\_and\_pad, resize\_to\_fill, resize\_to\_fit, resize\_to\_limit 是其中几个重要的图片处理的方法:
- - - -
resize_and_pad:  
Resize the image to fit within the specified dimensionswhile retaining the original aspect ratio.  
If necessary, will pad the remaining area with the given color, which defaults to transparent (for gif and png, white for jpeg).

大致的意思是，将图片resize到指定的大小，同时保留原来的宽高比。如果有必要会填充指定的颜色。
- - - -
resize_to_fill:  
Resize the image to fit within the specified dimensions while retaining the aspect   ratio of the original image. If necessary, crop the image in the larger dimension.  

大致的意思是，将图片resize到指定的大小，同时保持原始图像的宽高比。有可能拉伸图片到一个更大的尺寸
- - - -
resize_to_fit:  
Resize the image to fit within the specified dimensions while retaining the original aspect ratio.   
The image may be shorter or narrower than specified in the smaller dimension but will not be larger than the specified values.  

大致的意思是，将图片resize到指定的大小，同时保留原来的宽高比。图像可能是较短或较瘦的尺寸,但不会大于指定值。
- - - -
resize_to_limit:  
Resize the image to fit within the specified dimensions while retaining the original aspect ratio. Will only resize the image if it is larger than the specified dimensions.   
The resulting image may be shorter or narrower than specified in the smaller dimension but will not be larger than the specified values.  

大致的意思是，将图片resize到指定的大小，同时保留原来的宽高比,只会调整图像如果超过指定的尺寸。由此产生的图像可能是短或小于指定的小尺寸但不会大于指定值。
- - - -

参考文档  
 [CarrierWave Homepage] [1]
 [MiniMagick Rdoc] [2]
 [Blankyao's Blog] [3]

[1]: https://github.com/carrierwaveuploader/carrierwave "CarrierWave Homepage"
[2]: http://rubydoc.info/gems/carrierwave/frames "MiniMagick Rdoc"
[3]: http://blog.blankyao.com/story/resize_to_fit_and_resize_to_fill.html "http://blog.blankyao.com"
[Ray]:    http://mxbird.github.io  "Ray"

