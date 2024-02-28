将81164门课程进行清洗，调用老师给的脚本、同时trim掉空格！
```python
import html
import re
import pymongo
from pymongo import MongoClient
from html.parser import HTMLParser
from io import StringIO

class MLStripper(HTMLParser):
    def __init__(self):
        super().__init__()
        self.reset()
        self.strict = False
        self.convert_charrefs = True
        self.text = StringIO()

    def handle_data(self, d):
        self.text.write(d)

    def get_data(self):
        return self.text.getvalue()

def strip_html_tags(h):
    if(h == None):
        return ""
    h = html.unescape(h)
    h = re.sub(r'<script[^>]*>.*?</script>', '', h)
    h = re.sub(r'<style[^>]*>.*?</style>', '', h)
    s = MLStripper()
    s.feed(h)
    return s.get_data()

def clean_up_html(html_text):
    pure_text = strip_html_tags(html_text)
    fianl_text =  "".join(pure_text.split())
    return fianl_text

# 连接mongoDB
mongo_client = MongoClient("localhost", 27017)
# 选择一个数据库
db = mongo_client["MOOC123"]
# 选择一个数据表
collection = db["ScreeningCourseData"]
# 获取数据表中内容
dataList = collection.find()

[[创建一个数据表]]
new_collection = db["FinalCourseData"]

for data in dataList:
    # if(data["discribe"] == None):
    #     print(data["_id"])
    data["discribe"] = clean_up_html(data["discribe"])
    new_collection.insert_one(data)
```


实践后发现问题：
1. 有一些英文介绍，trime掉空格就...
2. 发现有describe字段为""的情况...





