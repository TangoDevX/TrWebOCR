alisen39大佬的项目，这里仅做clone备份，请访问https://github.com/alisen39/TrWebOCR

## 安装说明  
### 服务器部署
1. 安装python3.7  
    推荐使用miniconda
    
2. 安装依赖包  
``` shell script
pip install -r requirements.txt
```  

3. 运行  
项目默认运行在8089端口，默认不开启gpu：  
``` shell script
python backend/main.py [--port=8089][--open_gpu=0]
# --port 指定运行时端口号 默认是8089  
# --open_gpu 是否开启gpu 默认是0(不开启），可设置为1（开启）
```

看到以下输出则代表安装成功： 
```shell script
tr 2.3.0 https://github.com/myhub/tr
Server is running: http://192.168.31.95:8089
Now version is: cpu
```   

### Docker部署  
使用 Dockerfile 构建 或者直接 Pull镜像  
```shell script
# dockerfile 构建
docker build -t trwebocr:latest .

# 运行镜像
docker run -itd --rm -p 8089:8089 --name trwebocr trwebocr:latest 
```  

```shell script
# 从 dockerhub pull
docker pull mmmz/trwebocr:latest

# 运行镜像
docker run -itd --rm -p 8089:8089 --name trwebocr mmmz/trwebocr:latest 
```  
这里把容器的8089端口映射到了物理机的8089上，但如果你不喜欢映射，去掉run后面的`-p 8089:8089` 也可以使用docker的IP加`8089`来访问  

## 接口文档  
接口文档的内容放在了本项目的wiki里：  
[接口文档](https://github.com/alisen39/TrWebOCR/wiki/%E6%8E%A5%E5%8F%A3%E6%96%87%E6%A1%A3)    

## 接口调用示例  
* Python 使用File上传文件  
``` python
import requests
url = 'http://192.168.31.108:8089/api/tr-run/'
img1_file = {
    'file': open('img1.png', 'rb')
}
res = requests.post(url=url, data={'compress': 0}, files=img1_file)
```  

* Python 使用Base64  
``` python
import requests
import base64
def img_to_base64(img_path):
    with open(img_path, 'rb')as read:
        b64 = base64.b64encode(read.read())
    return b64
    
url = 'http://192.168.31.108:8089/api/tr-run/'
img_b64 = img_to_base64('./img1.png')
res = requests.post(url=url, data={'img': img_b64})
```

