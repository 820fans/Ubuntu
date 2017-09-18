1. 转单张图片为pdf
```
convert xxx.jpg test.pdf
```
2. 批量图片转换pdf及合并pdf
> 目的是将当前文件夹下的所有jpg图片转换进一个pdf；
> 本来用命令 convert *.jpg test.pdf 就可以将多个jpg图片转换进一个pdf文件了；
> 但是发生了 segmentation fault，解决方法；
> 下面采用的是一次转换一张jpg图片为pdf，再将多个pdf合并的方法。

### 转换所有jpg图片为pdf
```
$ find *.jpg -exec convert {} {}.pdf \\;
```
### 合并多个pdf
```
$ find *.jpg.pdf | xargs gs -q -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -sOutputFile=test.pdf
```
