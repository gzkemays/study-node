# Mybatis-plus 语句调用 

## 关于无法扫描到 xml 的问题解决方案：

### 一、修改 pom 为获取资源添加 xml。

```xml
<resources>
	<resource>
    	<directory>src/main/resource</directory>
        <filtering>true</filtering>
    </resource>
    <resource>
    	<directory>src/main/java</directory>
        <includes>
            <include>**/*.xml</include>
        </includes>
    </resource>
</resources>
```

### 二、将 xml 文件放入 resource 便于直接获取

## 简介

​    我们知道 mybatis-plus 是 mybatis 的扩展、优化，也就是说 mybatis 是基础。那么 mybatis-plus 就将简单的查询语句进行封装便于我们直接调用，但是对于表与表的直接关联查询，并没有很好的进行处理（ 对于 Jpa 而言 ）。它需要我们自己建立关联关系。在 jpa 中或许我们要形成关联只需要声明 @ManyToOne 、 @ManyToMany 或 @OneToOne 即可进行自动关联，但是 mybatis 则需要自己进行手动关联并建立联系，虽然一定程度上更加繁琐，但是却增加了代码的灵活性。

## 一对多的查询

- po 实体类：为属 “多” 的一方添加 List 实体类型，为 ”一“ 的一方为普通实体类型。

  - ```java
    // Blog 对于 Type 属于多的一方，一个博客只能属于一个分类，而一个分类可以拥有多个博客文章。
    @Data
    @EqualsAndHashCode(callSuper = false)
    public class TBlog implements Serializable {
        private static final long serialVersionUID=1L;
        @TableId(value = "id", type = IdType.AUTO)
        private Long id;
        private Boolean appreciation;
        private Boolean commentPower;
        private String content;
        @TableField(fill = FieldFill.INSERT)
        private Date createTime;
        private String firstPicture;
        private String flag;
        private Boolean recommend;
        private Boolean shareStatement;
        private String title;
        @TableField(fill = FieldFill.INSERT_UPDATE)
        private Date updateTime;
        private Integer views;
        private Long typeId;
        private Long userId;
        private String described;   
        // 一对多
        private TType type;
    }
    
    ```

  - ```java
    @Data
    @EqualsAndHashCode(callSuper = false)
    public class TType implements Serializable {
        private static final long serialVersionUID=1L;
        @TableId(value = "id", type = IdType.AUTO)
        private Long id;
        private String name;
        private List<TBlog> blogs;
    }
    ```

  

- 具体如何实现查询

  - 一、明确关联关系（在实体类中做相应的映射关系）

  - 二、对于 “一” —— 使用 association ， 对于 “多” —— 使用 collection

  - 三、使用 resultMap 声明需要返回的数据 （ 体现 mybatis 的灵活性）

    - 实现查询博客对应的分类

      - TBlogMapper.xml

      - ```xml
        <!-- 嵌套 findTypeById 进行查询，获取对应 Type 的信息。 -->
        <!-- 声明 id 的存在否则，会被传值进 select 方法后不做保存。 -->
        <resultMap id="blogWithTypeId" type="TBlog">
            <id column="id" property="id"/>
            <association property="type" javaType="TType" column="id" select="com.hwy.blog.mapper.TTypeMapper.findTypeById"/>
        </resultMap>
        
        <!-- 查询一个博客对应的分类 -->
        <select id="findTypeWithBlog" parameterType="Integer" resultMap="blogWithTypeId">
            select * from t_blog b where id = #{id}
        </select>
        ```
        
      - 对应的 SQL 语句：
      
        - ```sql
          select * from t_blog where id = ?  -- 获取该 blog 的所有信息
          ```
      
      - TTypeMapper.xml
      
      - ```xml
        <!-- 根据被调用的方法获取的 id 进行 Type 的查询，查询出的结果将被放入 blogWithTypeId 中。-->
        <select id="findTypeById" resultType="TType">
            select t.* from t_blog b , t_type t where b.type_id = t.id and b.id = #{id};
        </select>
        ```
      
      - 对应的 SQL 语句：
      
        - ```sql
          select t.* from t_blog b , t_type t where b.type_id = t.id and b.id = ? -- 根据 blog 的 id 获取所有 type 信息
          ```
      
    - <font color='red'>注重点：其特性在于获取的值是相同的，也就说，引出查询结果的条件是相同的 “blog” 的 id 值。</font>

## 多对一的查询

- po 实体类（与一对多相同）

- 具体如何实现查询：

  - 一、明确关联关系（在实体类中做相应的映射关系）

  - 二、对于 “一” —— 使用 association ， 对于 “多” —— 使用 collection

  - 三、返回的类型，当在 “一” 进行指定时，在 “多” 的地方不再用 javaType 来指定返回类型。因为此时返回的类型已经被 resultMap 所改变。

    - TBlogMapper.xml

    - ```xml
      <!-- 定义返回的 Blog 数据带有 Tags 标签 -->
      <resultMap id="blogTagsByType" type="TBlog">
          <collection property="tags" ofType="TTag" column="id" select="com.hwy.blog.mapper.TTagMapper.findTagById"/>
      </resultMap>
      
      <!-- 根据分类来获取博客文章 -->
      <select id="findBlogByType" parameterType="Integer" resultMap="blogTagsByType">
          select * from t_blog b where type_id = #{id}
      </select>
      ```

    - 对应 SQL 语句：

      - ```sql
        select * from t_blog b where type_id = #{id} -- 根据分类的 id 来查找对应的 blog
        ```

    - TTagMapper.xml

    - ```xml
      <select id="findTagById" parameterType="Integer" resultType="com.hwy.blog.pojo.TTag">
          select * from t_tag t where id in (select tags_id from t_blog_tags where blog_id = #{id})
      </select>
      ```

    - 对应 SQL 语句：

      - ```sql
        select * from t_tag t where id in (select tags_id from t_blog_tags where blog_id = ?) -- 查询 t_tag 表，它的 id 源自 t_blog_tags 中 blog_id 所对应的 tags_id。
        ```

    - TTypeMapper.xml

    - ```xml
      <resultMap id="typeResultMap" type="TType">
          <id column="id" property="id"/>
          <association property="blogs" column="id" select="com.hwy.blog.mapper.TBlogMapper.findBlogByType"/>
      </resultMap>
      <!-- 查询该标签下的所有文章 -->
      <select id="findBlogWithType" resultMap="typeResultMap">
          select b.title, t.* from t_blog b , t_type t where b.type_id = t.id and t.id = #{id};
      </select>
      ```

    - 对应 SQL 语句：

      - ```sql
        select b.title, t.* from t_blog b , t_type t where b.type_id = t.id and t.id = ? -- 根据 type 的 id 来查询所需要的数据，blog 中的 type_id 用于做间接数据。
        ```

  - <font color='red'> 注重点：</font>

    - <font color='red'> 一、返回类型是否使用了 resultMap 来做映射，如果使用了那么父查询则不需要定义 java 数据类型。 </font>  
    - <font color='red'> 二、多对一的关系中，“一” 的一方数据库表中，一般拥有对应的 id 外键来做映射，从而用来标明它们自己的联系。 </font>

## 多对多的查询

- po 实体类（TableField 声明该属性是数据库不具备的字段，但该属性是必要的）

  - ```java
    @Data
    @EqualsAndHashCode(callSuper = false)
    public class TBlog implements Serializable {
        private static final long serialVersionUID=1L;
        @TableId(value = "id", type = IdType.AUTO)
        private Long id;
        private Boolean appreciation;
        private Boolean commentPower;
        private String content;
        @TableField(fill = FieldFill.INSERT)
        private Date createTime;
        private String firstPicture;
        private String flag;
        private Boolean recommend;
        private Boolean shareStatement;
        private String title;
        @TableField(fill = FieldFill.INSERT_UPDATE)
        private Date updateTime;
        private Integer views;
        private Long typeId;
        private Long userId;
        private String described;
        // 多对多
        @TableField(exist = false)
        private List<TTag> tags;
        // 一对多
        @TableField(exist = false)
        private TType type;
    }
    ```

  - ```java
    @Data
    @EqualsAndHashCode(callSuper = false)
    @ToString
    public class TTag implements Serializable {
        private static final long serialVersionUID=1L;
        @TableId(value = "id", type = IdType.AUTO)
        private Long id;
        private String name;
        private List<TBlog> blogs;
    }
    ```

- 具体如何实现查询：

  - 多对多与一对多、多对一实际使用上是相同的，唯一不同的是形成多对多的关系同时，数据库中会用一个数据表来中纽带从而来标明它们之间的关联关系。

  - 因此我们只需要通过中间表来进行数据获取，便可以解决多对多的查询问题。

    - TBlogMapper.xml

    - ```xml
      <!-- 定义 resultMap 获取 blog 同时获取 tags 的数据 -->
      <resultMap id="blogWithTag" type="com.hwy.blog.pojo.TBlog">
          <id column="id" property="id"/>
          <collection property="tags" ofType="TTag" column="id" select="com.hwy.blog.mapper.TTagMapper.findTagById"/>
      </resultMap>
      
      <!-- 查询一个博客所拥有的标签 -->
      <select id="findTagWithBlog" parameterType="Integer" resultMap="blogWithTag">
          select * from t_blog b where id = #{id}
      </select>
      ```

    - 对应 SQL 语句：

      - ```sql
        select * from t_blog b where id = ? -- 获取一个博客所有的信息
        ```

    - TTagMapper.xml

    - ```xml
      <!-- 利用 in 来实现查询该博客下所有对应的 tags -->
      <select id="findTagById" parameterType="Integer" resultType="com.hwy.blog.pojo.TTag">
          select * from t_tag t where id in (select tags_id from t_blog_tags where blog_id = #{id})
      </select>
      ```

    - 对应的 SQL 语句：

      - ```sql
        select tags_id from t_blog_tags where blog_id = ? -- 根据 blog_id 来获取它对应的 tags_id 返回 ?,?,?
        select * from t_tag t where id in (?,?,?) -- 根据传入的 id 值来获取 t_tag 表中的所有数据
        ```

  - <font color='red'>注重点：多对多查询的注重点在于理解 resultMap 的嵌套数据查询的插入方式，像本查询中，resultMap (blogWithTag) 传值 id 给 findTagById 实现获取对应 blog 所需的 tags 列表。让 List\<TTag> tags 接收。从而实现获取 blog 所有信息同时获取对应的 tags 信息。</font>