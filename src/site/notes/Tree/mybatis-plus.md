---
{"dg-publish":true,"permalink":"/tree/mybatis-plus/","tags":["CS/programming-languages/java/java-frameworks"],"created":"2023-05-29T20:25:10.357+08:00","updated":"2023-08-27T03:49:53.113+08:00"}
---


以角色管理模块为例 用MP实现CURD的过程

- 第一步创建数据库和角色表
- 第二步项目创建Spring Boot配置文件
- 第三步创建角色实体类
- 第四步创建mapper接口，继承mp封装接口
-  第五步创建Spring Boot启动类 

```sql
CREATE TABLE `sys_role` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '角色id',
  `role_name` varchar(20) NOT NULL DEFAULT '' COMMENT '角色名称',
  `role_code` varchar(20) DEFAULT NULL COMMENT '角色编码',
  `description` varchar(255) DEFAULT NULL COMMENT '描述',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  `is_deleted` tinyint(3) NOT NULL DEFAULT '0' COMMENT '删除标记（0:不可用 1:可用）',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8 COMMENT='角色';
```

# CRUD

## basic
```java
@SpringBootTest

public class TestMpDemo1 {

  // 注入
  @Autowired
  private SysRoleMapper mapper;
  // 查询所有记录 
  @Test
  // must: import org.junit.jupiter.api.Test;
  // 注释直接写在注解后会报错
  public void getAll(){
    List<SysRole>  list = mapper.selectList(null);
    System.out.println(list);
  }

  // 添加操作
  @Test
  public void add(){
    SysRole sysRole = new SysRole();
    sysRole.setRoleName("角色管理员1");
    sysRole.setRoleCode("role1");
    sysRole.setDescription("角色管理员1");

    int  rows = mapper.insert(sysRole);
    System.out.println(rows);
    System.out.println(sysRole.getId());
  }

  // 修改
  @Test
  public void update(){
    // 根据ID查询
    SysRole role = mapper.selectById(10);
    // 设置修改值
    role.setRoleName("cqupt管理员");
    // 调用方法实现最终修改
    int rows = mapper.updateById(role);
    System.out.println(rows);
  }

  // 删除操作（逻辑删除
  @Test
  public void deleteId(){
    int rows = mapper.deleteById(10);
    System.out.println(rows);
  }
  // 批量删除
  @Test
  public void deleteBatchIds(){
    int result = mapper.deleteBatchIds(Arrays.asList(1, 2));
    System.out.println(result);
  }
}
```

## 条件查询

![diagram-of-mp-conditional-query.png](https://gcore.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-mp-conditional-query.png)

```java
 @Test
  public void testQuery1(){
    //创建QueryWrapper对象，调用方法封装条件
    QueryWrapper<SysRole> wrapper = new QueryWrapper<>();
    wrapper.eq("role_name","角色管理员");
    //调用mp方法实现查询操作
    List<SysRole> list = mapper.selectList(wrapper);
    System.out.println(list);
  }
  @Test
  public void  testQuery2(){
    //创建QueryWrapper对象，调用方法封装条件
    LambdaQueryWrapper<SysRole> wrapper = new LambdaQueryWrapper<>();
    wrapper.eq(SysRole::getRoleName,"角色管理员");
    //调用mp方法实现查询操作
    List<SysRole> list = mapper.selectList(wrapper);
    System.out.println(list);
  }
```

# 封装service层

```java
// SysRoleService.java
public interface SysRoleService extends IService<SysRole> {
}

...

//SysRoleServiceImpl.java
@Service
public class SysRoleServiceImpl extends ServiceImpl<SysRoleMapper,SysRole> implements SysRoleService {
}

...

//TestMpDemo2.java

@SpringBootTest

public class TestMpDemo2 {

  // 注入
  @Autowired
  private SysRoleService service;
  // 查询所有记录 
  @Test
  // must: import org.junit.jupiter.api.Test;
  // 注释直接写在注解后会报错
  public void getAll(){
    List<SysRole>  list = service.list();
    System.out.println(list);
  }
}

```

# 自动分页

-  配置分页插件

- 编写controller分页方法
	1. 需要参数：分页相关参数（当前页和每页显示记录数）
	2. 需要参数：条件参数 
- 调用service方法实现条件分页查询


 # 代码生成

引入依赖

```xml
	<dependency>
		<groupId>com.baomidou</groupId>
		<artifactId>mybatis-plus-generator</artifactId>
		<version>3.4.1</version>
	</dependency>

	<dependency>
		<groupId>org.apache.velocity</groupId>
		<artifactId>velocity-engine-core</artifactId>
		<version>2.0</version>
	</dependency>
```

使用工具类