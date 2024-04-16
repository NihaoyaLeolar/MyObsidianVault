---
tags:
  - MongoDB
---
使用python代码直接访问mongodb的方法
# [文档](https://pymongo.readthedocs.io/en/stable)
----
tutorial 部分写的特别清楚，可以快速使用上手
# 安装环境
----
```python
pip3 install pymongo
```

# 基本使用
-----
```python
import pymongo
from pymongo import MongoClient

# 连接mongoDB
mongo_client = MongoClient("localhost", 27017)

# 选择一个数据库
db = mongo_client["MOOC123"]

# 创建or选择一个数据表
collection = db["CoursesBackup-2023-06-08"]

# 获取数据表中内容
dataList = collection.find()
```


# 创建一个新表
----
这里以类的形式定义表的格式，并指定主键。如果没有指定主键，则会自动生成_id
```python
import pymongo
from pymongo import MongoClient

class ScreeningCourse:
    def __init__(self, id, name, discribe):
        self.id = id
        self.name = name
        self.discribe = discribe
    # 将_id字段设置为了我们指定的值
    # 这样在插入数据时就会使用我们指定的_id作为主键，而不是自动生成ObjectId
    def to_dict(self):
        return {
            "_id": self.id,     [[如果没有]]
            "name": self.name,
            "discribe": self.discribe
        }


# 连接mongoDB
mongo_client = MongoClient("localhost", 27017)
# 选择一个数据库
db = mongo_client["MOOC123"]
# 选择一个数据表
collection = db["CoursesBackup-2023-06-08"]
# 获取数据表中内容
dataList = collection.find()

[[创建一个数据表]]
new_collection = db["ScreeningCourseData"]

for data in dataList:
    course = ScreeningCourse(data["_id"], 
						     data["CourseName"], 
						     data["ContentInfo"][0]["Describe"])
    new_collection.insert_one(course.to_dict())
```
