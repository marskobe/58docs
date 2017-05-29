TOPIC&ACTIVITY&Info-API-DOC
===

## 话题&活动&资讯-API-移动端接口文档


### 所属

|项目 |地址 |模块 |访问 |备注 |负责人 |
|---- |:---|:---|:----:|:----:|:----:|
|mism-api |http://svn.58corp.com/tac |tac-api-provider |http://meishi.58v5.cn/tac |api 接口项目/话题&活动管理 |王志鹏 |

### 数据结构

#### TopicActivityInfo: 话题&活动&资讯

|属性名 |类型 |注释 |
|---- |:----|:----|
|id   |Long |主键	|
|type   |Integer |类型：1   话题；2 活动;3 资讯|
|title   |String |话题/活动/资讯的标题	  |
|cover   |String |话题/活动/资讯的封面	  |
|summary   |String |话题/资讯的简介	  |
|infoPublisher   |String |咨询的发布人	  |
|activityStartTime   |Date |活动开始时间|
|activityEndTime   |Date |活动结束时间	  |
|linkUrl   |String |活动/咨询的H5链接地址|
|sendRange   |Integer |发送范围：1  总部圈、2  销售圈、3  全部	  |
|readedNum   |Integer |话题/活动/资讯的阅读量	  |
|readedNumSimple   |String |readedNum的简写形式，例如"1.5万"	  |
|commentNum   |Integer |话题/活动/资讯的评论量	  |
|commentNumSimple   |String |commentNum的简写形式，例如"9999"		  |
|allowShared   |Integer |是否允许被分享：1 是；2 否	  |
|createdTime   |Date |创建时间	  |


#### Comment: 评论

|属性名 |类型 |注释 |
|---- |:----|:----|
|id   |Long |主键	|
|type   |Integer |评论主体的类型：1   话题；2 活动；3 咨讯	|
|taiId   |Long |被评论的话题/活动/资讯ID	|
|bspId   |Integer |评论者bspId	|
|oaName   |String |评论者OA名	|
|realName   |String |评论者真实姓名	  |
|headPicture   |String |评论者头像地址	  |
|time   |Date |评论时间	  |
|timeSimple   |Date |time的简写形式，例如：刚刚、5分钟前、1小时前、3天前	  |
|praiseNum   |Integer |被点赞数	  |
|praiseNumSimple   |Integer |praiseNum的简写形式，例如"1.5万"	  |
|content   |String |评论内容	  |

#### SlideActivity: 轮播活动

|属性名 |类型 |注释 |
|---- |:----|:----|
|id   |Long |主键	|
|title   |String |轮播活动的标题	  |
|image   |String |轮播活动的图片	  |
|linkUrl   |String |轮播活动的链接	  |
|position   |Integer |轮播活动的展位：1 ；2 ；3 ；4 ；5|
|status   |Integer |数据状态：1 有效；2 无效	  |
|created_time   |Date |创建时间	  |

## #TOPIC&ACTIVITY 话题&活动&资讯

### 接口(v10)

#### 查询话题&活动&资讯列表

##### 请求

```
GET http://meishi.58v5.cn/tac/topics
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----|
|endId |上一页的最后一条记录的ID |Yes/Null |根据endId取对应的createdTime,然后取小于它的10条数据；如果为Null则取最新的10条数据 |
|sortBy |首页热门话题时传hot |No/Null |值为hot时，查询最readedNum/createdTime最大的10条"话题"数据，不足10条取全部 |

##### 示例

```
$ curl -X GET "http://meishi.58v5.cn/tac/topics?endId=861821341806911488&sortBy=hot"
```
##### Return 结果
    
```
{
  "code": "1",
  "msg": "操作成功",
  "ctime": 1493963646945,
  "ciphertext": 1,
  "data": {
    "num": 1,
    "size": 2,
    "totalPages": 2,
    "content": [
      {
        "id": "3",
        "type": 1,
        "title": "wangba to 58",
        "cover": "http://imgs.58.com/3",
        "summary": "welcome to 58, i love you",
        "infoPublisher": "kobe",
        "activityStartTime": 1493454746000,
        "activityEndTime": 1494059583000,
        "linkUrl": "http://activity.58.com/100",
        "sendRange": 2,
        "allowShared": 11,
        "readedNum": 20000,
        "readedNumSimple": "2.0万",
        "commentNum": 20501,
        "commentNumSimple": "2.1万",
        "createdTime": 1493541330000
      },
      {
        "id": "1",
        "type": 1,
        "title": "i love you liudehua",
        "cover": "http://imgs.58.com/2",
        "summary": "welcome to 58, i love you",
        "infoPublisher": "kobe",
        "activityStartTime": 1493107321000,
        "activityEndTime": 1493280124000,
        "linkUrl": "http://activity.58.com/100",
        "sendRange": 1,
        "allowShared": 1,
        "readedNum": 10500,
        "readedNumSimple": "1.0万",
        "commentNum": 50,
        "commentNumSimple": "50",
        "createdTime": 1493454916000
      }
    ]
  },
  "extData": null
}
```

##### 结果说明
```
readedNumSimple 和 topicCommentNumSimple 分别为“阅读数”、“评论数”的简写形式。例如：20501 ----> 2.1万
```

### 接口(v10)

#### 查询话题&活动&资讯 （会级联出相关的评论，相关评论支持分页获取）

##### 请求

```
GET http://meishi.58v5.cn/tac/topics/id?id={id}&sortBy={sortBy}&endId={endId}
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----|
|id |话题&活动的id | Yes |int 限制大小(min>0) |
|sortBy |关联评论的排列顺序 | No/new |new 最新；hot 最热 |
|endId |上一页"评论"的最后一条记录的ID |Yes/Null |根据endId取对应的createdTime/praiseNum,然后取小于它的10条数据,如果为null则取最新的/最热的10条数据 |

##### 示例

```
$ curl -X GET "http://meishi.58v5.cn/tac/topics/id?id=1&sortBy=new&endId=860302926545969152"
```
##### Return 结果
    
```
{
  "code": "1",
  "msg": "操作成功",
  "ctime": 1493963922244,
  "ciphertext": 1,
  "data": {
    "id": "1",
    "type": 1,
    "title": "i love you liudehua",
    "cover": "http://imgs.58.com/2",
    "summary": "welcome to 58, i love you",
    "infoPublisher": "kobe",
    "activityStartTime": 1493107321000,
    "activityEndTime": 1493280124000,
    "linkUrl": "http://activity.58.com/100",
    "sendRange": 1,
    "allowShared": 1,
    "readedNum": 10500,
    "readedNumSimple": "1.0万",
    "commentNum": 50,
    "commentNumSimple": "50",
    "createdTime": 1493454916000,
    "comments": [
      {
        "id": "3",
        "topicId": 1,
        "bspId": "aa10086",
        "oaName": "wangzhipeng02",
        "realName": "王志鹏",
        "headPicture": null,
        "time": 1493691875000,
        "timeSimple": "2017-05-02 10:24:35",
        "praiseNum": 20555,
        "praiseNumSimple": "2.1万",
        "content": "i heate you"
      },
      {
        "id": "1",
        "topicId": 1,
        "bspId": "aa10086",
        "oaName": "wangzhipeng02",
        "realName": "王志鹏",
        "headPicture": null,
        "time": 1524563132000,
        "timeSimple": "刚刚",
        "praiseNum": 50,
        "praiseNumSimple": "50",
        "content": "hello 58ganji"
      }
    ]
  },
  "extData": null
}

```

##### 结果说明

```
注意使用这几个简写属性：readedNumSimple 、topicCommentNumSimple、timeSimple、praiseNumSimple
```

### 接口(v10)

#### 搜索话题&活动&资讯

##### 请求

```
GET http://meishi.58v5.cn/tac/topics/action/search?title=d&type=1
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----|
|title |标题 | Yes/ |会模糊查询标题中包含title的话题/活动/资讯|
|type |评论主体的类型：1   话题；2 活动；3 咨讯 |No | |

##### 示例

```
$ curl -X GET http://meishi.58v5.cn/tac/topics/action/search?title=d&type=1
```
##### Return 结果
    
```
{
  "code": "1",
  "msg": "操作成功",
  "ctime": 1495415217957,
  "ciphertext": 1,
  "data": [
    {
      "id": "861821341806911488",
      "type": 1,
      "title": "jjtestsdfasdfasdfasdf",
      "cover": "http://pic1.58cdn.com.cn/nowater/tacapi/n_v1bj3gzsckleivswxi24xq.png",
      "summary": "asdfasdfadfadfsafdddddddddddddddddddd",
      "infoPublisher": "",
      "activityStartTime": null,
      "activityEndTime": null,
      "linkUrl": "",
      "sendRange": 2,
      "allowShared": 1,
      "readedNum": 394,
      "readedNumSimple": "394",
      "commentNum": 0,
      "commentNumSimple": "0",
      "createdTime": "2017-05-09 13:53:15",
      "comments": null
    },
    {
      "id": "861820400395378688",
      "type": 1,
      "title": "asdfasdfasdfasdfasdf",
      "cover": "",
      "summary": "asdfasdfasdfasdfasdfasdfasdfasdfsdfasdfasdf",
      "infoPublisher": "",
      "activityStartTime": null,
      "activityEndTime": null,
      "linkUrl": "",
      "sendRange": 1,
      "allowShared": 1,
      "readedNum": 145,
      "readedNumSimple": "145",
      "commentNum": 0,
      "commentNumSimple": "0",
      "createdTime": "2017-05-09 13:49:35",
      "comments": null
    }
  ],
  "extData": null
}
```

### 接口(v10)

#### 查看话题&活动&资讯详情页

##### 请求

```
GET http://meishi.58v5.cn/h5/tac/topic
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----|
|type |评论主体的类型：1 话题；2 活动；3 咨讯 |Yes/1 | |
|clientType |设备类型 |Yes | |
|msKey |安全认证信息 |Yes | |
|msData |h5页面需要的参数（加密数据） |Yes | |
|userName |当前登录的用户名 |Yes | |

##### 示例

```
$ curl -X POST http://meishi.58v5.cn/h5/tac/topic?type={type} \
			   -d "type=1&
				   clientType=mobile&
				   msKey=aLV0lrZRC0bdTwufv8W28vOxWzgnyIiOWs3rR3xE4lor2rReVsyMN6ufS6aLTOMkdMgU27OaG%2BQc%2BgtXJp4%2FFC4%2BVWq5ythiTjEFaYhrDv9Vq9EIoZMBOHGW1pylMYTC2Tv5uoIvINqAYcVlr6dqPwB%2BNyX3mseGovQcdx7kdfYrMMHHKvDJ8v0mVXNOQ8z7EzkATvWegfUXMFipafE2covErWx%2BJh7evKKcE4wQJWhVONNGwKQ5Z2yJ1xiX%2Fq%2F0aOWHITwHP0ELpGzPfNm4L7r%2F0JO33mw5C0Nn4uFpJyw%3D"&
				   msData=AbrMy8D0GpSUXIGU7TPA6hFmVFHJVwQVmVllYdMzRF0sqoPKN%2F5DWE%2B2rsUG7cc2IlLTCiEs6huLuDLXfO%2FT5bK8DpmBVlMUqsl4ZwO40QM%3D&
				   userName=liuzhen06"
```
##### Return 结果
    
```

```

## #COMMENT 评论

### 接口(v10)

#### 添加评论

##### 请求

```
POST http://meishi.58v5.cn/tac/comments
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----:|
|taiId |被评论的话题/活动/资讯ID |Yes | |
|type |评论主体的类型：1   话题；2 活动；3 咨讯 |Yes | |
|content |评论内容 |Yes | |
|bspId |评论者的bspId | Yes | |


##### 示例

```
$ curl -X POST http://meishi.58v5.cn/tac/comments \
       -d "bspId=201112221050231e623c25&content=HelloWorld&topicId=1"
```

##### Return 结果

```
{
  "data": {
    null
  },
  "msg": "操作成功",
  "ctime": 1493121451575,
  "extData": null,
  "ciphertext": 1,
  "code": "1"
}
```

### 接口(v10)

#### 点赞评论

##### 请求

```
POST http://meishi.58v5.cn/tac/comments/action/praise
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----:|
|id |被点赞评论的id | Yes |int 限制大小(min>0) |
|bspId |点赞者的bspId | Yes | |

##### 示例

```
$ curl -X POST http://meishi.58v5.cn/tac/comments/action/praise \
       -d "id=1&bspId=201112221050231e623c25"
```

##### Return 结果

```
{
  "code": "1",
  "msg": "操作成功",
  "ctime": 1494817575398,
  "ciphertext": 1,
  "data": {
    "commentId": "1",
    "hasPraised": true,
    "praiseNum": 10
  },
  "extData": null
}
```
##### 结果说明

```
如果某人已经点赞过这条评论，那么将增加一个字段 "hasPraised": true，点赞数不变；否则该属性为空。
```

### 接口(v10)

#### 查询评论列表

##### 请求

```
GET http://meishi.58v5.cn/tac/comments
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----:|
|page.num |页号 |No/1 |int 限制大小(min>0) |
|page.size |页大小 |No/20 |int 限制大小(min>0) |
|sort.property |排序属性 |No/ |例如：praise_num/time |
|sort.direction |排序顺序 |No/ |枚举：ASC/DESC（只能大写） |
|comment |评论对象 |No/ |根据comment对象不为空的属性进行过滤 |

##### 示例

```
$ curl -X GET "http://meishi.58v5.cn/tac/comments?page={num:1,size:2}&sort={property:"praise_num",direction:"DESC"}"
```
##### Return 结果
    
```
{
  "code": "1",
  "msg": "操作成功",
  "ctime": 1493694320226,
  "ciphertext": 1,
  "data": {
    "num": 1,
    "size": 2,
    "totalPages": 7,
    "content": [
      {
        "id": "3",
        "topicId": 1,
        "bspId": "aa10086",
        "oaName": "wangzhipeng02",
        "realName": "王志鹏",
        "headPicture": "https://pic1.58cdn.com.cn/nowater/mis/n_v1bl2lwxshljzvq6k2ryla.jpg",
        "time": 1493691875000,
        "timeSimple": "40分钟前",
        "praiseNum": 20555,
        "praiseNumSimple": "2.1万",
        "content": "i heate you"
      },
      {
        "id": "2",
        "topicId": 2,
        "bspId": "aa10086",
        "oaName": "wangzhipeng02",
        "realName": "王志鹏",
        "headPicture": "https://pic1.58cdn.com.cn/nowater/mis/n_v1bl2lwxshljzvq6k2ryla.jpg",
        "time": 1490696609000,
        "timeSimple": "2017-03-28 18:23:29",
        "praiseNum": 8888,
        "praiseNumSimple": "8888",
        "content": "i love you"
      }
    ]
  },
  "extData": null
}
```

##### 结果说明

```
注意：timeSimple、praiseNumSimple，分别为time、praiseNum的简写形式
```

## #SLIDEACTIVITY 轮播活动

### 接口(v10)

#### 查询轮播活动列表

##### 请求

```
GET http://meishi.58v5.cn/tac/slideactivities
```

|参数 |说明 |必须/默认值 |备注 |
|---- |:---|:---|:----:|
|无 |会查询出所有有效的轮播活动 |No/ |有效活动最多5条 |

##### 示例

```
$ curl -X GET "http://meishi.58v5.cn/tac/slideactivities"
```
##### Return 结果
    
```
{
  "code": "1",
  "msg": "操作成功",
  "ctime": 1493719819351,
  "ciphertext": 1,
  "data": [
    {
      "id": "1",
      "title": "元旦放假消息通知",
      "image": "http://img.58.com/slideActivity/1",
      "linkUrl": "http://activity.58.com/1",
      "position": 1,
      "status": 1,
      "createdTime": null
    },
    {
      "id": "2",
      "title": "清明放假消息通知",
      "image": "http://img.58.com/slideActivity/2",
      "linkUrl": "http://activity.58.com/2",
      "position": 2,
      "status": 1,
      "createdTime": null
    },
    {
      "id": "3",
      "title": "端午放假消息通知",
      "image": "http://img.58.com/slideActivity/1",
      "linkUrl": "http://activity.58.com/1",
      "position": 1,
      "status": 1,
      "createdTime": null
    },
    {
      "id": "4",
      "title": "中秋放假消息通知",
      "image": "http://img.58.com/slideActivity/1",
      "linkUrl": "http://activity.58.com/1",
      "position": 1,
      "status": 1,
      "createdTime": null
    },
    {
      "id": "5",
      "title": "国庆放假消息通知",
      "image": "http://img.58.com/slideActivity/1",
      "linkUrl": "http://activity.58.com/1",
      "position": 1,
      "status": 1,
      "createdTime": null
    }
  ],
  "extData": null
}
```