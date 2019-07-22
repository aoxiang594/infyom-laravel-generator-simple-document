# InfyOm Laravel Generator Simple Document

InfyOm Laravel Generator是一个基于Laravel的，自动生成CURL、API Router、 Model、 Requests等的轮子。

## 官方文档
[http://labs.infyom.com/laravelgenerator/docs](http://labs.infyom.com/laravelgenerator/docs)


## 安装
根据Laravel版本不同，安装不同版本的Laravel Generator
```
composer require infyomlabs/laravel-generator  5.5.x-dev
```
版本号可以在
[https://packagist.org/packages/infyomlabs/laravel-generator](https://packagist.org/packages/infyomlabs/laravel-generator)


## 使用
介绍两种方式，推荐方式二

要生成一个帖子增删改查改查的API路由、model等
### 方式一:基于命令行模式生成
```
#Post就是要生成的模型、路由的名字
> php artisan infyom:api Post
#输入完上面命令以后会弹出下面对话，这里有4个选项
#Field: (name db_type html_type options)
```

| name |  description |
| --- |  --- |
|  name|  数据库字段名称  |
|  db_type|  数据库字段类型，例如string，text |
|html_type|这个是输出的类型|
|options|指是否是搜索字段、fillable等|

具体查看
[http://labs.infyom.com/laravelgenerator/docs/5.5/getting-started#field-inputs](http://labs.infyom.com/laravelgenerator/docs/5.5/getting-started#field-inputs)

```
#下面是我的示例，不是需要自己输入的命令
Specify fields for the model (skip id & timestamp fields, we will add it automatically)
Read docs carefully to specify field inputs)
Enter "exit" to finish

 Field: (name db_type html_type options) []:
 > title string string

 #requests中设置的选项，这里我只设置必填
 Enter validations:  []:
 > required 

 Field: (name db_type html_type options) []:
 > content string string

 Enter validations:  []:
 > required

 Field: (name db_type html_type options) []:
 > views int number

 Enter validations:  []:
 > required

 Field: (name db_type html_type options) []:
 > exit

#生成相关数据库迁移、模型、Requests、路由等文件
Migration created: 
2019_07_22_013951_create_posts_table.php

Model created: 
Post.php

Repository created: 
PostRepository.php

Create Request created: 
CreatePostAPIRequest.php

Update Request created: 
UpdatePostAPIRequest.php

API Controller created: 
PostAPIController.php

posts api routes added.

RepositoryTest created: 
PostRepositoryTest.php

TestTrait created: 
MakePostTrait.php

ApiTest created: 
PostApiTest.php

 #是否立即执行数据库迁移，我一般会否，然后自己手动修改一下数据库迁移文件
Do you want to migrate database? [y|N] (yes/no) [no]:
 > n

Generating autoload files
```



### 方式二:基于配置文件生成
在`config\infyom\laravel_genertor.php`中有一个`schema_files`参数，定义了schema配置文件存放位置，一般都在`resources/model_schemas/`
在这个文件夹里新增一个`PostSchema.json`的文件。配置内容稍后讲解。
配置完成以后，执行命令
```
php artisan infyom:api_scaffold Post --fieldsFile=PostSchema.json
```

配置简易说明

| name | description |  
| --- | --- |
| name |数据库字段名称  |
|dbType|数据库字段类型,例如:string:nullable:comment,'密码混淆'|
|htlmType|输入类型|
|validations|requests 验证内容、下面会给案例|
|searchable|搜索|
|fillable|create的时候允许插入的字段|
|primary|主键|
|inForm|具体我也说不清楚|
|inIndex|具体我也说不清楚|
|type|relation  模型关系|
|relation|1tm就是1对多咯，mtm多对多，然后是关系连接字段|

看一个例子
```
[
  {
    "name": "id",
    "dbType": "increments",
    "htmlType": "",
    "validations": "",
    "searchable": false,
    "fillable": false,
    "primary": true,
    "inForm": false,
    "inIndex": false
  },
  {
    "name": "title",
    "dbType": "string",
    "htmlType": "text",
    "validations": "required",
    "searchable": true
  },
  {
    "name": "post_date",
    "dbType": "dateTime",
    "htmlType": "date",
    "searchable": true
  },
  {
    "name": "body",
    "dbType": "text",
    "htmlType": "textarea"
  },
  {
    "name": "password",
    "dbType": "string",
    "htmlType": "password",
    "searchable": false,
    "inForm": false,
    "inIndex": false
  },
  {
    "name": "salt",
    "dbType": "string:nullable:comment,'密码混淆'",
    "htmlType": "text",
    "searchable": false,
    "inForm": false,
    "inIndex": false
  }
  {
    "name": "token",
    "dbType": "string",
    "htmlType": "hidden",
    "searchable": false,
    "inForm": false,
    "inIndex": false
  },
  {
    "name": "email",
    "dbType": "string",
    "htmlType": "email",
    "searchable": true
  },
  {
    "name": "author_gender",
    "dbType": "integer",
    "htmlType": "radio,Male:1,Female:0"
  },
  {
    "name": "post_type",
    "dbType": "string",
    "htmlType": "radio,Public,Private",
    "searchable": true
  },
  {
    "name": "post_visits",
    "dbType": "integer",
    "htmlType": "number"
  },
  {
    "name": "category",
    "dbType": "string",
    "htmlType": "select,Technology,LifeStyle,Education,Games",
    "searchable": true
  },
  {
    "name": "category_short",
    "dbType": "string",
    "htmlType": "select,Technology:tech,LifeStyle:ls,Education:edu,Games:game"
  },
  {
    "name": "is_private",
    "dbType": "boolean",
    "htmlType": "checkbox,1"
  },
  {
    "name": "writer_id",
    "dbType": "integer:unsigned:default,0:foreign,writers,id",
    "htmlType": "text",
    "relation": "mt1,Writer,writer_id,id"
  },
  {
    "type": "relation",
    "relation": "1tm,Comment"
  },
  {
    "type": "relation",
    "relation": "1tm,User:customRelationName,user_id,id"
  },
  {
    "name": "users",
    "type": "relation",
    "relation": "mtm,Role,user_roles,user_id,role_id"
  },
  {
    "name": "created_at",
    "dbType": "timestamp",
    "htmlType": "",
    "validations": "",
    "searchable": false,
    "fillable": false,
    "primary": false,
    "inForm": false,
    "inIndex": false
  },
  {
    "name": "updated_at",
    "dbType": "timestamp",
    "htmlType": "",
    "validations": "",
    "searchable": false,
    "fillable": false,
    "primary": false,
    "inForm": false,
    "inIndex": false
  }
]

```


## 最后
如果觉得生成的数据有问题
可以执行回滚，这样会删除所有生成的文件。
```
php artisan infyom:rollback Post api
```

`生成数据库迁移文件以后，最好自己先看一遍，再执行migrate`
