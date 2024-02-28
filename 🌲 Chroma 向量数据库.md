# 一些概念
----
在实现时，只要关注两个对象：

- `client`：连接向量数据库的客户端对象
- `collection`：向量数据库中的数据表

具体API使用直接看官方文档即可，很简短～

collection 就是表，document 和 vector 是并列的条（vector 可以理解为一种索引）

![](https://cdn.nlark.com/yuque/0/2023/png/25369650/1686207681947-eaa58d77-3569-44f5-bf55-fc6a091fb62d.png)

## Embedding/向量
embedding 是 document的向量化描述
有三种方法获得：
1. 像你代码里一样自己计算传进去
2. chromadb 自带一个 SentenceTransformer 模型，自动计算 documents 的 embedding
3. 也可以指定 chromadb 的 embedding 模型，比如给你的 api 用的是 multilingual-e5-base，支持多语言