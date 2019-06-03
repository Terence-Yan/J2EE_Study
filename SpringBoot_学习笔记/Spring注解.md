#### 1.@Autowired
* 自动注入注解：首先它会根据属性类型查找对应的唯一的Bean，如果对应类型的Bean不是唯一的，那么它会根据其属性名称和Bean的名称进行匹配。如果匹配得上，就
会使用该Bean；如果还无法匹配，就会抛出异常。
* @Autowired是一个默认必须找到对应Bean的注解，如果不能确定其标注属性一定会存在并且允许这个被标注的属性为null，那么可以将@Autowired的属性required设
置为false，如`@Autowired(required = false)`。
