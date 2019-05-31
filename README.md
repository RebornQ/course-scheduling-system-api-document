# 《日理万机》机房排课系统API文档

API设计为RESTFUL风格

已使用swagger-ui自动生成API文档，地址为：[http://www.tinsfox.com:8081/course-scheduling-system/swagger-ui.html#/](http://www.tinsfox.com:8081/course-scheduling-system/swagger-ui.html#/)

BaseUrl：[http://www.tinsfox.com:8081/course-scheduling-system](http://www.tinsfox.com:8081/course-scheduling-system)

## 用户增删查改操作
### 注册
- 接口URL：`/user`
- Method：`POST`
- 提交的参数（参数类型为`raw`，提交json数据）

    ```json
    {
      "academyNo": "002",
      "password": "12345678",
      "userNo": "001",
      "username": "Test"
    }
    ```
    > 说明：`academyNo`为学院编号，`userNo`为个人编号，`username`为用户名，`password`为密码
    
- 返回的内容
  - 成功
  
    ```json
    {
       "code": 200,
       "message": "注册成功！",
       "data": {
          "userName": "Test",
          "userNumber": "2019002001"
        }
    }
    ```
        
    > 说明：`userName`为用户名，`userNumber`为帐号
        
### 登录
- 接口URL：`/user/login`
- Method：`POST`
- 提交的参数（参数类型为`raw`，提交json数据）

    ```json
    {
      "password": "12345678",
      "userNumber": "2019002001"
    }
    ```
    > 说明：`userNumber`为帐号，`password`为密码
    
- 返回的内容
  - 成功
    
    ```json
    {
       "code": 200,
       "message": "登录成功！",
       "data": {
          "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NTQ4OTg3NjQsInVzZXJOdW1iZXIiOiIyMDE5c3RyaW5nc3RyaW5nIn0.gtXOaYtRIcys8SVGU9lki5AlLdL_LzOo05LNIYHCz7Y"
       }
    }
    ```
    > 说明：
    > `token`为登录后该用户的Token，**登录后的需要验证用户的操作都需要在`Header`带上该Token**。
    > **例：`{ "key": "Authorization", "value":"获取到的token"}`**
        
## 课室的各种操作
### 查询某个机房的信息
- 接口URL：`/room/info`
- Method：`POST`
- 提交的参数（参数类型为`form-data`/`x-www-form-urlencoded`，提交表单数据）

    ```
    "building": "B5"
    "number": "203"
    ```
    > 说明：`building`为建筑编号，`number`为房间号
    
- 返回的内容
  - 成功
    
    ```json
    {
      "code": 200,
      "message": "查询成功！",
      "data": {
        "id": 1,
        "building": "B5",
        "number": 203,
        "name": "基础实验室一室",
        "count": 43,
        "introduction": null
      }
    }
    ```
    > 说明：`building`为建筑编号，`number`为房间号，`name`为课室名称，`count`为学生可容纳的学生数量，`introduction`为课室介绍

### 查询所有机房的信息
- 接口URL：`/room/info/all`
- Method：`GET`
- 返回的内容
  - 成功
    
    ```json
    {
      "code": 200,
      "message": "查询成功！",
      "data": [
        {
          "id": 1,
          "building": "B5",
          "number": 203,
          "name": "基础实验室一室",
          "count": 43,
          "introduction": null
        },
        {
          "id": 2,
          "building": "B5",
          "number": 204,
          "name": "基础实验室二室",
          "count": 43,
          "introduction": null
        }
      ]
    }
    ```
    > 说明：`building`为建筑编号，`number`为房间号，`name`为课室名称，`count`为学生可容纳的学生数量，`introduction`为课室介绍
        
### 获取当前时间需要开门的课室列表
- 接口URL：`/room/openlist`
- Method：`GET`
- 参数：type(可选)

  > 默认获取当前时间**正在使用**的课室列表，参数可选填写，若为1则获取当前时间**需要开门**的课室列表
  
- 请求链接举例：
    - 不带参数：`.../room/openlist`
    - 带参数：`.../room/openlist?type=1`
- 返回的内容
  - 成功
   
    ```json
    {
        "code": 200,
        "message": "查询成功！",
        "data": [
            {
                "roomNo": "B5-204",
                "courseName": "C语言",
                "teacher": "里包恩",
                "studentNum": 43,
                "week": 7,
                "weekDay": 3,
                "timePeriodId": 2,
                "needOpenDoor": true
            },
            {
                "roomNo": "B5-203",
                "courseName": "C语言",
                "teacher": "里包恩",
                "studentNum": 45,
                "week": 7,
                "weekDay": 3,
                "timePeriodId": 2,
                "needOpenDoor": false
            }
        ]
    }
    ```
    > 说明：`roomNo`为课室编号，`courseName`为课程名，`teacher`为老师名，`studentNum`为该节所需的座位数(即所来学生数)，`week`为第几周，`weekDay`为星期几，`timePeriodId`为课程时间段，`needOpenDoor`为是否需要开门。
        
### 查询某个机房的使用信息列表
- 接口URL：`/room/info/used`
- Method：`POST`
- 提交的参数（参数类型为`form-data`/`x-www-form-urlencoded`，提交表单数据）

    ```
    "building": "B5"
    "number": "203"
    ```
    > 说明：`building`为建筑编号，`number`为房间号
    
- 返回的内容
  - 成功
    
   
    ```json
    {
      "code": 200,
      "message": "查询成功！",
      "data": [
        {
          "id": 3,
          "week": 7,
          "weekDay": 3,
          "timePeriodId": 2,
          "roomId": 1,
          "courseId": 3,
          "mvalue": ""
        },
        {
          "id": 4,
          "week": 7,
          "weekDay": 3,
          "timePeriodId": 1,
          "roomId": 1,
          "courseId": 3,
          "mvalue": ""
        }
      ]
    }
    ```
    > 说明：
    > 
    > | 字段名 | 字段描述 |
    > | --- | --- |
    > | week | 周数 |
    > | weekDay | 星期几 |
    > | timePeriodId | 课程时间段 |
    > | weekDay | 星期几 |
    > | roomId | 课室唯一id |
    > | courseId | 对应的已计划课程id |




## 课室申请相关操作
### 添加课室申请
- 接口URL：`/room/application`
- Method：`POST`
- 提交的参数（参数类型为`raw`，提交json数据）

    ```json
    {
      "academicYear": "2018-2019",
      "building": "B5",
      "courseInfo": {
        "academy": "计算机学院",
        "creditHours": "4",
        "major": "软件工程",
        "name": "C语言",
        "nature": "必修课",
        "type": "专业基础课"
      },
      "courseNo": "string",
      "roomNumber": 203,
      "stuCount": "43",
      "stuCreditHours": "4",
      "teacher": "里包恩",
      "term": 1,
      "timePeriodId": 1,
      "type": 0,
      "week": 3,
      "weekDay": 5
    }
    ```
    > 说明：
    > 
    > | 字段名 | 字段描述 |
    > | --- | --- |
    > | academicYear | 学年 |
    > | building | 建筑编号 |
    > | academy | 学院 |
    > | creditHours | 学时 |
    > | major | 专业 |
    > | name | 课程名 |
    > | nature | 课程性质 |
    > | courseInfo.type | 课程类型 |
    > | courseNo | 课程号（不是课程id，确认申请前无课程id） |
    > | roomNumber | 房间号 |
    > | stuCount | 学生人数 |
    > | stuCreditHours | 人学时 |
    > | teacher | 任课老师 |
    > | term | 学期 |
    > | timePeriodId | 课程时间段 |
    > | type | 申请类型 |
    > | week | 周数 |
    > | weekDay | 星期几 |
    
- 返回的内容
  - 成功
   
    ```json
    {
        "code": 200,
        "message": "添加成功！",
        "data": 1
    }
    ```

### 获取课室申请列表
- 接口URL：`/room/applications`
- Method：`GET`
- 返回的内容
  - 成功
   
    ```json
    {
        "code": 200,
        "message": "查询成功！",
        "data": [
            {
                "id": 4,
                "content": {
                    "building": null,
                    "roomNumber": 203,
                    "week": 3,
                    "weekDay": 5,
                    "timePeriodId": 1,
                    "courseInfo": {
                        "id": 0,
                        "name": "C语言",
                        "nature": "必修课",
                        "type": "专业基础课",
                        "academy": "计算机学院",
                        "major": "软件工程",
                        "creditHours": "4",
                        "notes": null
                    },
                    "academicYear": "2018-2019",
                    "term": 1,
                    "courseNo": "string",
                    "teacher": "里包恩",
                    "stuCount": "43",
                    "stuCreditHours": "4",
                    "type": 0
                },
                "type": 0,
                "status": false
            }
        ]
    }
    ```
    
    > 说明：
    > 
    > | 字段名 | 字段描述 |
    > | --- | --- |
    > | academicYear | 学年 |
    > | building | 建筑编号 |
    > | academy | 学院 |
    > | creditHours | 学时 |
    > | major | 专业 |
    > | name | 课程名 |
    > | nature | 课程性质 |
    > | courseInfo.type | 课程类型 |
    > | courseNo | 课程号（不是课程id，确认申请前无课程id） |
    > | roomNumber | 房间号 |
    > | stuCount | 学生人数 |
    > | stuCreditHours | 人学时 |
    > | teacher | 任课老师 |
    > | term | 学期 |
    > | timePeriodId | 课程时间段 |
    > | type | 申请类型 |
    > | week | 周数 |
    > | weekDay | 星期几 |
 
