---

---
---
![[CleanShot 2023-11-06 at 16.35.38@2x.png|400]]
# 数据格式问题
---
北师大给出的数据是csv的数据表，但是数据库一般使用utf8编码。
**要确保导入源文件和数据库的表的编码方式匹配**！
在mac上的操作：
1. 使用textedit打开csv文件
![[CleanShot 2023-11-06 at 16.30.43@2x.png|500]]、
2. 复制一份
![[CleanShot 2023-11-06 at 16.34.15@2x.png|400]]
3. 保存复制的那一份时，更改文本编码方式
![[CleanShot 2023-11-06 at 16.32.44@2x.png|300]]