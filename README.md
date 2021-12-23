## 实验环境准备
注意：目前可以查询到的pytorch版本都不支持windows上的python2，所以建议在linux环境下进行该实验
* Python 2.7+ （Python3上面测试过，会报错，建议使用Python2）
* [PyTorch 0.4.0+](http://pytorch.org)
* [qpth 0.0.11+](https://github.com/locuslab/qpth)
* [tqdm](https://github.com/tqdm/tqdm)

## 使用

### 安装以及实验步骤

1.复制仓库代码:
    ```bash
    git clone https://github.com/yilisita/MAML.git
    cd MAML
    ```
2. 数据集：miniImage.zip，解压对应的文件，建议是放在data路径下面

3. 在/data/mini_imagenet.py 的第30行配置好数据集所在的路径:
    ```python
    _MINI_IMAGENET_DATASET_DIR = 'miniImageNet'
    ```

### 元训练
1. SVM-Based 的 5-way 元训练对应的命令（这里只做了miniImage数据集的实验）:
    ```bash
    python train.py --gpu 0,1,2,3 --save-path "./experiments/miniImageNet_MetaOptNet_SVM" --train-shot 15 \
    --head SVM --network ResNet --dataset miniImageNet --eps 0.1
    ```
2. '--head' 参数可以修改为 "ProtoNet"，"Ridge"等值表示使用不同的模型来提取任务信息.


### 元测试
1. 5-way 1-shot miniImage 测试:
```
python test.py --gpu 0,1,2,3 --load ./experiments/miniImageNet_MetaOptNet_SVM/best_model.pth --episode 1000 \
--way 5 --shot 1 --query 15 --head SVM --network ResNet --dataset miniImageNet
```
2. 5-way 5-shot miniImage 测试:
```
python test.py --gpu 0,1,2,3 --load ./experiments/miniImageNet_MetaOptNet_SVM/best_model.pth --episode 1000 \
--way 5 --shot 5 --query 15 --head SVM --network ResNet --dataset miniImageNet
```
