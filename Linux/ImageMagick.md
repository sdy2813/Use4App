# ImageMagick 常见操作

## 批量转换文件夹中图片的格式
```bash
for i in *.png
 do 
 i=${i%.png*}
convert $i.png ${i}.jpg
done
```





---
参考资料
1. [ImageMagick 入门教程](https://www.jianshu.com/p/310d833d9a25)

1. [ImageMagick 教程](https://www.w3cschool.cn/imagemagick_use/)
