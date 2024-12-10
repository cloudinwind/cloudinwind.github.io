
**pytorch 环境安装**：

```shell
conda install pytorch==1.10.0 torchvision==0.11.0 torchaudio==0.10.0 cudatoolkit=11.3 -c pytorch
```

> 其他版本不保证能够成功

**源码下载：**
```shell
git clone https://github.com/PyRetri/PyRetri.git
cd PyRetri
```

**源码修改：**

修改 `setup.py` 中的 `sklearn` 为 `scikit-learn`
```python
if __name__ == '__main__':
    write_version_py()
    setup(
        name='pyretri',
        version=get_version(),
        description='A Toolbox for Deep Learning-based Image Retrieval',
        long_description=readme(),
        author='Megvii & XJTU',
        author_email='https://github.com/???',
        keywords='computer vision, image retrieval',
        url='https://github.com/???',
        classifiers=[
            'Development Status :: 4 - Beta',
            'License :: OSI Approved :: Apache Software License',
            'Operating System :: OS Independent',
            'Programming Language :: Python :: 3',
            'Programming Language :: Python :: 3.5',
            'Programming Language :: Python :: 3.6',
            'Programming Language :: Python :: 3.7',
        ],
        license='Apache License 2.0',
        install_requires=[
            'numpy', 'torch>=1.2', 'torchvision>=0.4', 'scikit-learn', 'yacs', 'tqdm',
        ],
        packages=find_packages(),
        zip_safe=False)
```


**环境配置：**

```shell
python3 setup.py install
```

必须进行下面配置：
```shell
pip uninstall pyretri
```

使用编译环境：
```shell
pip install -e .
```


**修改模型加载方式：**

修改 `pyretri/utils/misc.py` 文件中的 
`from torchvision.models.utils import load_state_dict_from_url

为 
`from torch.hub import load_state_dict_from_url`

> 在 0.11.0 的 torchvision 版本中, 必须使用 torch.hub



**训练时保证数据在同一个设备**

`workspace/PyRetri/main/extract_feature.py`

```python
def main():

    # init args
    args = parse_args()
    assert args.data_json is not None, 'the dataset json must be provided!'
    assert args.save_path is not None, 'the save path must be provided!'
    assert args.config_file is not None, 'a config file must be provided!'
    
    # 设置设备
    device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

    # init and load retrieval pipeline settings
    cfg = get_defaults_cfg()
    cfg = setup_cfg(cfg, args.config_file, args.opts)

    # build dataset and dataloader
    dataset = build_folder(args.data_json, cfg.datasets)
    dataloader = build_loader(dataset, cfg.datasets)

    # 构建模型，并将模型移动到指定的设备
    model = build_model(cfg.model)
    model = model.to(device)

    # 构建helper并提取特征
    extract_helper = build_extract_helper(model, cfg.extract)

    # 提取特征
    extract_helper.do_extract(dataloader, args.save_path, args.save_interval)

```

`workspace/PyRetri/pyretri/extract/builder.py`

```python
def build_extract_helper(model: nn.Module, cfg: CfgNode) -> ExtractHelper:
    """
    Instantiate a extract helper class.

    Args:
        model (nn.Module): the model for extracting features.
        cfg (CfgNode): the configuration tree.

    Returns:
        helper (ExtractHelper): an instance of extract helper class.
    """
    assemble = cfg.assemble
    extractor = build_extractor(model, cfg.extractor)
    splitter = build_splitter(cfg.splitter)
    aggregators = build_aggregators(cfg.aggregators)
    helper = ExtractHelper(assemble, extractor, splitter, aggregators)
    return helper
```


`workspace/PyRetri/pyretri/extract/extractor/extractors_base.py`

```python
def __init__(self, model: nn.Module, feature_modules: Dict[str, nn.Module], hps: Dict or None = None):
        """
        Args:
            model (nn.Module): the model for extracting features.
            feature_modules (dict): the output layer of the model.
            hps (dict): default hyper parameters in a dict (keys, values).
        """
        super(ExtractorBase, self).__init__(hps)
        assert len(self._hyper_params["extract_features"]) > 0

        self.model = model.eval()
#         if torch.cuda.is_available():
#             self.model.cuda()
#             if torch.cuda.device_count() > 1:
#                 self.model = nn.DataParallel(self.model)
        device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
        self.model = self.model.to(device)
        
                
        self.feature_modules = feature_modules
        self.feature_buffer = dict()

        if self._hyper_params["extract_features"][0] == "all":
            self._hyper_params["extract_features"] = self.available_feas
        for fea in self._hyper_params["extract_features"]:
            self.feature_buffer[fea] = dict()

        self._register_hook()
```


`workspace/PyRetri/pyretri/extract/helper/helper.py`

```python
def __init__(self, assemble: int, extractor: ExtractorBase, splitter: SplitterBase, aggregators: List[AggregatorBase]):
        """
        Args:
            assemble (int): way to assemble features if transformers produce multiple images (e.g. TwoFlip, TenCrop).
            extractor (ExtractorBase): a extractor class for extracting features.
            splitter (SplitterBase): a splitter class for splitting features.
            aggregators (list): a list of extractor classes for aggregating features.
        """
        self.assemble = assemble
        self.extractor = extractor
        self.splitter = splitter
        self.aggregators = aggregators
        
        
        device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
        self.device = device

def extract_one_batch(self, batch: Dict) -> Dict:
        """
        Extract features for a batch of images.

        Args:
            batch (dict): a dict containing several image tensors.

        Returns:
            all_fea_dict (dict): a dict containing extracted features.
        """
        img = batch["img"]
#         if torch.cuda.is_available():
#             img = img.cuda()
        img = img.to(self.device)
    
        # img is in the shape (N, IMG_AUG, C, H, W)
        batch_size, aug_size = img.shape[0], img.shape[1]
        img = img.view(-1, img.shape[2], img.shape[3], img.shape[4])

        features = self.extractor(img)

        features = self.splitter(features)
		all_fea_dict = dict()
        for aggregator in self.aggregators:
            fea_dict = aggregator(features)
            all_fea_dict.update(fea_dict)

        # PyTorch will duplicate inputs if batch_size < n_gpu
        for key in all_fea_dict.keys():
            if self.assemble == 0:
                features = all_fea_dict[key][:img.shape[0], :]
                features = features.view(batch_size, aug_size, -1)
                features = features.view(batch_size, -1)
                all_fea_dict[key] = features
            elif self.assemble == 1:
                features = all_fea_dict[key].view(batch_size, aug_size, -1)
                features = features.sum(dim=1)
                all_fea_dict[key] = features

        return all_fea_dict
```