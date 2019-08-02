# https://docs.conda.io/en/latest/miniconda.html python下载

# PyCharm 编辑器

```
添加清华镜像源，代码如下所示：
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
```

# 关于解决cv2.findContours返回值too many values to unpack的问题

```
binary, contours, opt = cv2.findContours(bwimg, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
```