# pdftk基本操作

## 软件安装
```bash
sudo apt-get install pdftk
```
```bash
## 基本操作
# 提取pdf的1至10页生成一个新的pdf
pdftk *.pdf cat 1-10 output range.pdf

## 提取第1至3，第5，第6至10页，并合并为一个pdf文件
$ pdftk input.pdf cat 1-3 5 6-10 output combined.pdf

#拆分PDF的每一页为一个新文件 并按照指定格式设定文件名
pdftk input.pdf burst output new_%d.pdf

#按照通配符，合并大量PDF文件
pdftk *.pdf cat output combined.pdf

#去除第 13 页,其余的保存为新PDF
pdftk in.pdf cat 1-12 14-end output out1.pdf

#按180°旋转所有页面
pdftk input.pdf cat 1-endsouth output output.pdf

#按顺时针90°旋转第三页，其他页不变
pdftk input.pdf cat 1-2 3east 4-end output output.pdf

# 修改PDF的文件结构（目录）
# 提取PDF的目录结构为一个txt文件
pdftk sample.pdf dump_data output info.txt

# 手动修改txt文件中的目录结构
# ...

# 将txt文件重新加载到PDF中并生成一个新文件
$ pdftk sample.pdf update_info info.txt output sample2.pdf

# PDF 128 位加密，保留全部权限:
pdftk 1.pdf output 1.128.pdf owner_pw foopass

# PDF 128 位加密，保留全部权限，打开文档需输入密码 “baz”:
pdftk *.pdf output *_128.pdf owner_pw foo user_pw baz

# 压缩 PDF:
pdftk *.pdf output compressed.pdf compress

# 提前准备好水印PDF(water.pdf)样式
pdftk *.pdf stamp water.pdf output *_stamp_all_page.pdf
```
