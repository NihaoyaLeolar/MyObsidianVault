将数据库的所需字段(id, name, describe)筛选出来，只保留`IsHeadCourse=true`的课程。同时保证所有字段都不为空。

- 仍存储在原集合`MOOC123`下，新表名称：`ScreeningCourseData`
- 主键设置为课程id
![[CleanShot 2023-08-07 at 10.25.28 1.png]]

疑问：
1. 没有describe字段的课程：`data["ContentInfo"]==null`/`data["ContentInfo"][0]=null`。直接丢弃or存入其他的描述？   -> 直接舍掉
2. 有describe字段，但是是空字符串     -> 直接舍掉

# 代码
----
```python
# Author: WangBin
# 将数据库的所需字段筛选出来，只保留IsHeadCourse=true的8w+课程

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
            "_id": self.id,
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

number_of_courses_with_missing_conditions = 0
for data in dataList:
    # 跳过
    if(data["IsHeadCourse"] == False):
        continue    
    # 有些课程没有data["ContentInfo"][0]这个字段
    if(len(data["ContentInfo"]) == 0 or data["ContentInfo"][0]["Describe"] == None):
        number_of_courses_with_missing_conditions += 1
        continue
    course = ScreeningCourse(data["_id"], 
						     data["CourseName"], 
						     data["ContentInfo"][0]["Describe"])
    new_collection.insert_one(course.to_dict())

print("原课程总数:", collection.count_documents({}))
print("插入课程总数:", new_collection.count_documents({}))
print("没有data['ContentInfo'][0]这个字段而被刷掉的课程数:", number_of_courses_with_missing_conditions)

# 大概跑了10分钟，结果如下：
# 原课程总数: 483007
# 插入课程总数: 83126
# 没有data['ContentInfo'][0]这个字段而被刷掉的课程数: 111
```