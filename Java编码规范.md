1. 编码规范遵循[《阿里巴巴Java开发手册》](https://developer.aliyun.com/topic/java2020)，代码格式遵循[《Google Java Style》](https://google.github.io/styleguide/javaguide.html)。
2. 代码中涉及文件路径问题时，必须使用文件路径分隔符(File.separator)代替正斜杠(/)或反斜杠(\\)。
3. DTO类和Entity类实现Serializable接口，并且定义serialVersionUID为默认值 1L。
4. 批量更新多条数据的时候，不允许先删除数据再重新插入，要使用新数据集合和旧数据集合对比，计算出需要新增、修改和删除的数据后，再分别执行数据库操作。
5. Service和Controller对同一个业务模型的列表、创建、查询、更新、删除方法只能存在一个实际实现方法，可以基于方法重载实现功能相同参数不同的情况，避免出现大量重复的代码。
6. Controller的REST地址遵循RESTful风格。
7. 代码中涉及字符、数字、日期、数组、集合等对象操作时，优先使用Apache Commons或Google Guava的工具类，如以下情况：
    - 判断字符不为null或空，使用StringUtils.isNotBlank
    - 去除字符首尾的空格，使用StringUtils.trimToEmpty
    - 字符串转换为数字，使用NumberUtils.toLong、NumberUtils.toInt等
    - 判断Boolean对象是否为True，使用BooleanUtils.isTrue(flag)
    - 根据字符转换为枚举类型，使用EnumUtils.getEnum(TargetType.class, type)
    - 判断集合不为null且元素大于0，使用CollectionUtils.isNotEmpty
8. Spring中Bean实例注入，使用构造函数注入，不要使用字段注入，避免类之间形成循环引用。
9. 关于JPA的使用
    - 不允许使用findOne方法
10. 关于Java方法
    - 不允许在方法中改变输入参数的值
    - 不允许修改调用其他方法的返回值，如往集合类返回值中添加或移除元素
    - 必须校验输入参数，并注意是否需要移除字符类参数前后的空格
    - 如果返回值为集合类型，不要返回null，可以使用Collections.emptyList代替
11. 关于变量
    - 变量名称必须是有意义的单词或通用缩写，注意IDE提示的拼写错误
    - 局部变量的作用域最小化，如使用循环方法时临时变量要定义在循环体内
12. 关于基本数据类型和包装数据类
    - DTO类或Entity类的属性定义使用包装数据类型
    - 方法体内局部变量使用基本数据类型
    - 普通方法的参数和返回值使用基本数据类型
    - RPC方法的参数和返回值使用包装数据类型
13. 关于集合
    - 不允许在不判断集合元素数量的情况下获取第一个元素，如代码中`return list.get(0) ;` 或`Object object = list.iterator().next();`等
    - 如果调用方法返回的是集合对象，不可直接对其进行添加或删除元素的操作
14. 关于注释
    - 所有Java类和接口内方法必须有完善的Javdoc格式注释
    - 方法体内核心逻辑要有单行或多行注释
15. 关于代码清理
    - 提交代码前必须移除多引用的包，可以使用IDE的Optimize Imports功能实现
    - 必须及时删除类中不需要注入的类和不再使用的方法
16. 其他
    - 不要将lombok用于生成getter和setter方法之外的其他地方，如不要使用lombok的@slf4j、@NonNull注解
    - 使用slf4j输出日志和异常信息，禁止使用`System.out`和`e.printStackTrace()`方式输出
    - @ConditionalOnClass只能用于类上才生效