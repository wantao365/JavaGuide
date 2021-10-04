# 多对一关联映射

多个用户属于一个组:User----Group

User                             Group
-----------          -----------
id:int                            id:int
name:String               name:String
group:Group

关联映射的本质:
	将关联关系映射到数据库, 所谓的关联关系是对象模型在内存中的一个或 多个引用

```xml
<many-to-one name="group" column="groupid" casecade="save-update">
		
</many-to-one>
```



# 一对一主键关联映射-单向

一个人只有一个身份证:Person---->IDCard

Person                 IDCard
-----------           -----------
id:int                     id:int
name:String        cardNo:String
idCard：IDCard


单向时指：站在Person这一段可以看到IDCard对象
双向是指：站在Person这一段可以看到IDCard对象同时站在IDCard对象一段页可以看到对用的Person对象

主键关联：让关联对象的id（主键）保持一致，不用加任何字段；就是让Person的主键和IDCard保持一致，而不是自动增长

IDCard.hbm.xml

```xml
<class name="com.softeem.IDCard">
    <id name="id">
        <generator class="native"/>
    </id>
    <property name="cardNo"/>
</class>
```

Person.hbm.xml

```xml
<class name="com.softeem.Person">
    <id name="id">
        <generator class="foreign"> :设置成外键关联
            <param name="property">idCard</param>:关联icCard对应主键
        </gennerator> 
    </id>
    <property name="name"/>
    <one-to-one name="idCard" constrained="true"/>
	添加上外键约束：指示hibernate怎样加载他的关联对象（默认根据主键加载）
	一对一主键关联映射中，默认了casecade属性
</class>
```



# 一对一主键关联映射-双向

一个人和一个身份证只能唯一对应:Person<---->IDCard

Person                IDCard
-----------       -----------
id:int                       id:int
name:String          cardNo:String
idCard：IDCard    person:Person


双向是指：站在Person这一段可以看到IDCard对象同时站在IDCard对象一段页可以看到对用的Person对象

主键关联：让关联对象的id（主键）保持一致，不用加任何字段；就是让Person的主键和IDCard保持一致，而不是自动增长

IDCard.hbm.xml

```xml
<class name="com.softeem.IDCard">
    <id name="id">
        <generator class="native"/>
    </id>
    <property name="cardNo"/>
    <one-to-one name="person">
</class>
```

Person.hbm.xml

```xml
<class name="com.softeem.Person">
    <id name="id">
        <generator class="foreign"> :设置成外键关联
            <param name="property">idCard</param>:关联icCard对应主键
        </gennerator> 
    </id>
    <property name="name"/>
    <one-to-one name="idCard" constrained="true"/>
	添加上外键约束：指示hibernate怎样加载他的关联对象（默认根据主键加载）
	一对一主键关联映射中，默认了casecade属性
</class>
```



# 一对一唯一外键关联映射-单向

一个人只有一个身份证:Person---->IDCard

Person            IDCard
-----------       -----------
id:int                   id:int
name:String       cardNo:String
idCard：IDCard

单向时指：站在Person这一段可以看到IDCard对象

外键关联：在一张表中添加一个外键，指向关联表的主键（在Person表中添加外键指向IDCard的主键）

```xml
IDCard.hbm.xml
-----------------------------------------------
<class name="com.softeem.IDCard">
    <id name="id">
        <generator class="native"/>
    </id>
    <property name="cardNo"/>
</class>


Person.hbm.xml
-----------------------------------------------
<class name="com.softeem.Person">
    <id name="id">
        <generator class="native"/>
    </id>
    <property name="name"/>
    一对一就是多对一中的特例：给外键条件了一个唯一性约束
    <many-to-one name="idCard" unique="true"/>
</class>
```



# 一对一唯一外键关联映射-双向

一个人只有一个身份证:Person<---->IDCard

Person               IDCard
-----------       -----------
id:int                   id:int
name:String       cardNo:String
idCard：IDCard    person:Person

外键双向关联：在一张表中添加一个外键，指向关联表的主键（在Person表中添加外键指向IDCard的主键）

```xml
IDCard.hbm.xml
-----------------------------------------------

<class name="com.softeem.IDCard">
    <id name="id">
        <generator class="native"/>
    </id>
    <property name="cardNo"/>
    一对一关联映射默认的是通过主键加载指定的对象
    添加了property-ref属性之后，就表示加载的时候就根据person对象的idCard加载，因为icard也是唯一的，所以实现了一对一的关联
    <one-to-one name="person" property-ref="idCard"/>
</class>


Person.hbm.xml
-----------------------------------------------

<class name="com.softeem.Person">
    <id name="id">
        <generator class="native"/>
    </id>
    <property name="name"/>
    一对一就是多对一中的特例：给外键条件了一个唯一性约束
    <many-to-one name="idCard" unique="true"/>
</class>
```

