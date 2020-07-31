# Hibernate 博客项目转换为 Mybatis-Plus

## 一、转换前的准备

- 关闭 DDL 注入更新。（在已定型的实体类已经没必要再进行更新处理）

  ```properties
  # 配置 ddl-auto ：
  # 1、create 删除已有的表,重新创建,退出时不删除表.
  # 2、create-drop 在 1 的基础上删除表(表不报错）
  # 3、update 如果启动时数据库表与po中定义的表不一致,则创建表,原数据保留.
  # 4、validate 项目启动表结构进行校验,如果不一致就报错.
  #spring.jpa.hibernate.ddl-auto=update
  #spring.jpa.show-sql=true
  #spring.jpa.properties.hibernate.enable_lazy_load_no_trans=true
  ```

- 实体类中 @Table 改成 @TableName，@TableName 是 MP 的一个注释用来扫描对应的数据库。
- 临时数据处理字段中的：@Transient 改成 @TableField 定义为必要而非数据库字段属性。
- 若要将 Hibernate POJO 转换成 Mybatis-Plus 所需要的 POJO 最好使用逆向工程。
  - 因为 Mybatis-Plus 并没有 @ManyToOne 的查询。
  - Mybatis-Plus 必须通过 SQL 语句来完成表与表之间的关联查询。

## 二、代码自动生成器

```java
package com.myself.blog.code;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.po.TableFill;
import com.baomidou.mybatisplus.generator.config.rules.DateType;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

import javax.sql.DataSource;
import java.util.ArrayList;
import java.util.List;

public class BlogCode {
    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();
        GlobalConfig gc = new GlobalConfig();
        // 获取当前目录
        String parentPath = System.getProperty("user.dir");
        // 指定生成目录
        gc.setOutputDir(parentPath+"/src/main/java");
        gc.setAuthor("gzkemays");
        gc.setOpen(false);
        gc.setFileOverride(false);
        gc.setDateType(DateType.ONLY_DATE);
        gc.setServiceName("%sService");
        // 全局配置
        mpg.setGlobalConfig(gc);
        
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://127.0.0.1:4096/blog?useSSL=false&serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8");
        dsc.setUsername("root");
        dsc.setPassword("gzkemays");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setDbType(DbType.MYSQL);
        // 数据源配置
        mpg.setDataSource(dsc);
        
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("mp_blog");
        pc.setParent("com.myself.blog");
        pc.setEntity("po");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setController("controller");
        // 配置包的生成规则
        mpg.setPackageInfo(pc);
        
        StrategyConfig sc = new StrategyConfig();
        // 包含的表名
        sc.setInclude("t_blog","t_tag","t_type","t_blog_tags","t_comment","t_user");
        // 下划线转换成驼峰
        sc.setColumnNaming(NamingStrategy.underline_to_camel);
        // 包的命名规则
        sc.setNaming(NamingStrategy.underline_to_camel);
        // 启用 Lombok
        sc.setEntityLombokModel(true);
        List<TableFill> tableFills = new ArrayList<>();
        TableFill createTime = new TableFill("create_time", FieldFill.INSERT);
        TableFill updateTime = new TableFill("update_time",FieldFill.INSERT_UPDATE);
        tableFills.add(createTime);
        tableFills.add(updateTime);
        sc.setTableFillList(tableFills);    
        // 生成策略
        mpg.setStrategy(sc);
        
        mpg.execute();
    }
}

```



