# torchKbert
- Our customized version of bert for pytorch

## 说明
这是笔者基于 Meelfy 的 pytorch_pretrained_BERT 库进行部分定制化修改的模型库。

本项目的初衷是为了满足个人实验的方便，因此不会经常更新。

## 功能
- 原始的模型库 <a href="https://github.com/Meelfy/pytorch_pretrained_BERT">pytorch_pretrained_BERT</a> 中的功能仍然支持；
- 支持<a href="https://spaces.ac.cn/archives/7947">层次分解位置编码</a>。
- 支持<a href="https://github.com/ZhuiyiTechnology/WoBERT">基于词颗粒度的woBERT</a>。
    Pytorch 权重（这里提供的是WoBERT Plus模型）：
    - <a href="https://pan.baidu.com/s/1uxfRby4GXi1EpWlvlL8VCQ">chinese_wobert_plus.zip</a>（提取码: fg6j)

## 使用
- 安装：
    ```shell
    pip install torchKbert
    ```

- 典型的使用例子请参考官方 <a href="https://github.com/Meelfy/pytorch_pretrained_BERT/tree/master/examples">examples</a> 目录。

- 若想使用层次分解位置编码，使 BERT 可以处理长文本，在 `model` 中传入参数 `is_hierarchical=True` 即可。示例如下：
    ```
    model = BertModel(config)
    encoder_outputs, _ = model(input_ids, token_ids, input_mask, is_hierarchical=True)
    ```

- 若想使用<a href="https://kexue.fm/archives/7758">基于词颗粒度的中文WoBERT</a>，只需在构建`BertTokenizer`对象时传入新参数：
    ```
    from torchKbert.tokenization import BertTokenizer

    tokenizer = BertTokenizer(
        vocab_file=vocab_path, 
        pre_tokenizer=lambda s: jieba.cut(s, HMM=False))
    ```
    
    不传入时，默认为`None`。分词时默认以词为单位，若想恢复使用以字为单位，只需在`tokenize`时传入新参数`pre_tokenize=False`：
    ```
    tokenzier.tokenize(text, pre_tokenize=False)
    ```


## 背景
之前一直在用 Meelfy 编写的 [pytorch_pretrained_BERT](https://github.com/Meelfy/pytorch_pretrained_BERT)，调用预训练模型或进行微调已经十分方便。后来因个人的需求，所以就想改写一个支持层次分解位置编码的版本。

苏神的 <a href="https://github.com/bojone/bert4keras">bert4keras</a> 已经实现了这样的功能。但因个人惯于使用 pytorch，已经很久不用 keras 了，所以才打算自己改写一个。

## 更新
- <strong> 2021.03.07 </strong>: 添加层次分解位置编码。
- <strong> 2021.05.27 </strong>: 添加基于词颗粒度的中文WoBERT。
- <strong> 2022.03.27 </strong>: 参照 [pytorch_transformers](https://github.com/abidlabs/pytorch-transformers) 对 BertPretrainedModel 代码实现进行了重构。

## 参考
- 感谢 Meelfy 实现的 [pytorch_pretrained_BERT](https://github.com/Meelfy/pytorch_pretrained_BERT)，本实现完全基于 pytorch_pretrained_BERT 的源码。
- 感谢苏神的 insight 和无私分享：[层次分解位置编码，让BERT可以处理超长文本](https://spaces.ac.cn/archives/7947)。
- [WoBERT: Word-based Chinese BERT model - ZhuiyiAI](https://github.com/ZhuiyiTechnology/WoBERT)。

