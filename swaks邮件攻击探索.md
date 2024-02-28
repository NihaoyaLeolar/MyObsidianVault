实验在kali中进行，自带swaks

# 基本使用
----
```xml
-- to <收件人的邮箱>
-- from <要显示的发件人邮箱>
-- ehlo <伪造的邮件ehlo头>
-- body <邮件正文>
-- header <邮件头信息，subject为邮件标题>
```
当要攻击的邮件平台没有SPF记录时，直接使用swaks伪造发送即可！

```shell
# 发件人伪装成 admin@ofse.com, 攻击老师的邮箱：laptergrsd@139.com
swaks --body "test" --header "Subject:HelloWorld" -t laptergrsd@139.com -f "admin@ofse.com"
```

而大部分正规平台使用了SPF记录来拦截伪造发送的行为
```shell
# qq会检测from邮箱和user是否一致!
swaks --to 1286746591@qq.com --from 1286746591@qq.com --server smtp.qq.com:465 --tls-on-connect --auth-user 1286746591@qq.com --auth-password dwkbrpstreygbadh

# 直接攻击谷歌/qq邮箱，会被SPF拦截，550错误
swaks --to xiaoxiaomeitou@gmail.com --from admin@ofse.com

```

如果存在SPF记录，则利用第三方邮件托管平台伪造，但是一般会显示由xxx代发
swaks --to qpzami02361@chacuo.net --from admin@ofse.com
# 绕过 SPF
## 利用邮件托管平台绕过SPF记录
- [smtp2go](https://www.smtp2go.com/)（速度慢但免费发送量大，需要自己有域名）
- [SendCloud](https://www.sendcloud.net/)（速度快但免费发送量少）:key-2ed826514c45df1170f6098b60091abe
- 也可以自己搭建邮件服务器
这里我们使用smtp2go，注册账户并登录，smtp2go中需要存在有效SMTP Users和Domains才能正常投递邮件。

```shell
swaks --to 1286746591@qq.com --from hr@huawei.com --header 'Subject: test main' --ehlo gmail.com --body 'hello, I am blut' --server sendcloud.org -au sc_3vyf8g -ap 2ed826514c45df1170f6098b60091abe --header-X-Mailer gmail.com
```

 不太行得通，留一个坑在这以后慢慢研究吧！