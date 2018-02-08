GitHub中的markdown文件直接写[TOC]是无法生成目录的，可以使用工具[doctoc](https://www.npmjs.com/package/doctoc)

## 步骤：

1. [安装node](https://www.npmjs.com/package/doctoc/tutorial)

  [下载地址](https://nodejs.org/download/release/latest/)

  ![](http://upload-images.jianshu.io/upload_images/9028834-81d91d426d885e46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 安装完node重启电脑

3. 安装doctoc
  CMD中输入：`npm install -g doctoc`
  ![](http://upload-images.jianshu.io/upload_images/9028834-a46746c8576a485c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 打开md文件目录
  ![](http://upload-images.jianshu.io/upload_images/9028834-57b58a3b6764e062.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 对当前文件夹中所有文件生成目录
  `doctoc .`

6. 对文件夹中单个文件生成目录（文件名中间不能有空格）
  `doctoc /path/to/file [...]`
  如：`doctoc README.md`