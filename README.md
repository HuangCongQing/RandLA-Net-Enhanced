<!--
 * @Description: 
 * @Author: HCQ
 * @Company(School): UCAS
 * @Date: 2020-05-24 12:25:19
 * @LastEditors: HCQ
 * @LastEditTime: 2020-05-24 12:48:09
--> 
# RandLA-Net-Enhanced

此代码基于[https://github.com/QingyongHu/RandLA-Net](https://github.com/QingyongHu/RandLA-Net)优化，下为此代码对应文献信息。


```latex
@article{hu2019randla,
  title={RandLA-Net: Efficient Semantic Segmentation of Large-Scale Point Clouds},
  author={Hu, Qingyong and Yang, Bo and Xie, Linhai and Rosa, Stefano and Guo, Yulan and Wang, Zhihua and Trigoni, Niki and Markham, Andrew},
  journal={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
  year={2020}
}
```




## 项目结构


项目结构如下
![image.png](https://cdn.nlark.com/yuque/0/2020/png/232596/1590232669705-548ef760-7040-4f2c-935e-1bb95ff563ee.png#align=left&display=inline&height=459&margin=%5Bobject%20Object%5D&name=image.png&originHeight=700&originWidth=456&size=52133&status=done&style=none&width=299)
### 重点文件说明



| **文件** | **作用** |
| --- | --- |
| helper_tf_util.py | 封装了一些卷积池化操作代码 |
| helper_tool.py | 有训练时各个数据集所用到的一些参数信息，还有一些预处理数据时的一些模块。 |
| main_SemanticKITTI.py | 训练对应数据的主文件 |
| RandLANet.py | 定义网络的主题结构 |
| tester_SemanticKITTI.py | 测试对应数据的文件，该文件在main_SemanticKITTI.py中被调用 |
| utils | 该文件夹里面有对数据集预处理的模块以及KNN模块。 |

### ☆Tips

- `jobs_test_semantickitti.sh` **在测试集上评估时，会在result文件夹中日期最近的文件夹内根据checkpoint记录寻找模型参数文件**


## 重点说明
 
压缩包下面有**四个文件夹分别对应报告中第三部分的四处尝试优化点**，下表为不同文件夹对应的优化点：

| **文件夹** | **优化点** |
| --- | --- |
| RandLA-Net-[4,4,4,2,2] | 使用五层网络的下采样优化 |
| RandLA-Net-[4,4,4,4,2] | 使用五层网络的下采样优化 |
| RandLA-Net-normals | 增添法向量估计 |
| RandLA-Net-square | 使用内积替换欧式距离 |



下面代码安装和运行方式步骤中，1-3步为通用步骤，**4-6步进入每个文件夹下都要对应执行一次。**



## 代码安装&运行方式
配置环境：Python 3.5, Tensorflow 1.11, CUDA 9.0 and cuDNN 7.3.1 on Ubuntu 16.04.

### 1. 仓库clone
```powershell
git clone --depth=1 https://github.com/HuangCongQing/RandLA-Net-Enhanced && cd RandLA-Net-Enhanced
```


### 2. python运行环境配置
```powershell
conda create -n randlanet python=3.5
source activate randlanet
conda install cudatoolkit=9.0 cudnn=7.3.1
pip install -r helper_requirements.txt
pip install tensorflow-gpu==1.11
```
### 3. SemanticKITTI数据集下载
 
下载链接：[http://semantic-kitti.org/dataset.html#download](http://semantic-kitti.org/dataset.html#download)
下载内容包含下图中4个文件包。下载完成后，并将其解压至`/data/semantic_kitti/dataset`

![](https://cdn.nlark.com/yuque/0/2020/png/232596/1590214973455-420b2721-ecaa-4396-8d02-a222b7705c1d.png#align=left&display=inline&height=489&margin=%5Bobject%20Object%5D&originHeight=489&originWidth=1484&size=0&status=done&style=none&width=1484)



### 4. 数据集预处理
```powershell
python utils/data_prepare_semantickitti.py
```
 
### 5. 模型训练
```powershell
python main_SemanticKITTI.py --mode train --gpu 0
```
 
### 6. 模型测试
```powershell
sh jobs_test_semantickitti.sh
```


