### 下载并安装NLTK包
1. 安装Anaconda3后就不需要安装nltk
	+ pip install nltk
2. 下载nltk_data----[连接地址](https://github.com/nltk/nltk_data)
	+ 下载Packages文件夹改名为 nltk_data
	+ 将包中部分压缩包解压(如punkt，不解压即使路径对了还是会报上面的错误)
3. 将nltk_data放入到nltk可以搜索到的路径中，或者直接将其当前路径添加到nltk搜索的路径列表中
```
from nltk import data
data.path.append(r"/home/hadoopcj/nltk_data(你的路径)")
```
4. 查看nltk_data的搜索路径
```
nltk.data.path
```