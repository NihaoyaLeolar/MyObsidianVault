---
tags:
  - springboot
---
在Spring框架中，`@Resource` 和 `@Autowired` 注解都可以**用来注入依赖**。

它们的作用很相似，但有些微小的区别：
- `@Autowired` 是Spring提供的注解，它是按照类型进行自动装配的。它会尝试根据类型去找到匹配的Bean进行注入。
- `@Resource` 是Java的注解，它是按照名称进行自动装配的。它会根据属性名或者`name`属性去寻找匹配的Bean进行注入。

如果以前用的是`@Autowired`，而现在改成了`@Resource`，那么您需要确保`StringRedisTemplate` Bean 在Spring容器中有唯一的名称，且与属性名一致，或者您可以使用`name`属性来指定具体的Bean名称。