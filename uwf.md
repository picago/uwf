FORMAT: 1A
HOST: http://uwf.apiblueprint.org/

# 工作流引擎 API

工作流引擎是基于炎黄bpm封装的,意在提供简单的api.

## 流程API

### 启动流程 [POST /inner/processService/start]

在流程设计器画好之后, 在界面上会有一个流程id,根据此id启动流程

+ Request (application/json)
    + Body

            [{
                "processDefId": "obc_111k3kk34",
                "title":"申请请假",
                "uid":"uxxx199",
            }]

    + Attributes (array[ProcParams])

        
        
+ Response 200 (application/json)
    + Body 
    
            {
             "head":{
                "errorCode":"0",
                "errorMsg":"success"
             },
             "body":{
                  "processInstId": "proc_xkdkfksdsk",
                  "controlState":"active
                  "createTime" : "2017-01-12 14:08:10",
                  "createUserLocation": "8349ae76-33e1-4174-94cd-ae1d0d54bf2c",
                  "createUserOrgId": "10c9f909-2504-4aa2-b1cf-df9d84b3a651",
                  "createUser": "lisi"
              }
            }
            
           
    + Attributes (object)

        + processInstId (string) - 流程实例id
        + createTime    (string) - 创建时间,格式 2017-04-12 13:01:08
        + controlState    (string) - 流程状态, 正常是active
        + createUserDeptId    (string) - 用户所处部门的id
        + createUserOrgId    (string) - 用户所在机构(单位)的id
        + createUser    (string) - 创建人



### 流程变量查询 [POST /inner/processService/queryVars]

流程变量查询

+ Request (application/json)
    + Body

            {
                "processInstId": "1333434"
            }

    + Attributes (object)

        + processInstId (string, required) - 流程实例id
        
+ Response 200 (application/json)
    + Body 
        
            {
             "head":{
                "errorCode":"0",
                "errorMsg":"success"
             }
             "body":{
                "vars": {
                 "k1": "v1",
                 "k2": 2,
                 "k3":3.8,
                 "k4": [1, 2, 3]   
                }
             }
            }
            
    + Attributes (object)

        + vars (object) - 流程变量

### 改变流程变量 [POST /inner/processService/setVars]

流程变量查询

+ Request (application/json)
    + Body

            {
                "processInstId": "1333434",
                "vars":{}
            }

    + Attributes (object)

        + processInstId (string, required) - 流程实例id
        + vars (object, required) - 流程变量
        
+ Response 200 (application/json)
    + Body 
        
            {
             "head":{
                "errorCode":"0",
                "errorMsg":"success"
             }
             "body":{
                "vars": {
                 "k1": "v1",
                 "k2": 2,
                 "k3":3.8,
                 "k4": [1, 2, 3]   
                }
             }
            }
            
    + Attributes (object)

        + vars (object) - 流程变量           

### 流程实例跟踪图 [POST /inner/processService/track]

不是一个流图片, 而是一个url

+ Request (application/json)
    + Body

            {
                "processInstId": "1333434",
                "sid": "token....skdfkdsk"
            }

    + Attributes (object)

        + processInstId (string, required) - 流程实例id
        + sid (string, required) - 登录时候拿到的token
        
+ Response 200 (application/json)
    + Body 
        
            {
             "head":{
                "errorCode":"0",
                "errorMsg":"success"
             }
             "body":{
                "url": "http://xxx.yyy.zzz/skksk"
             }
            }
            
    + Attributes (object)

        + url (string) - url       

### 流程查询 [POST /inner/processService/query]

根据条件来查询流程

+ Request (application/json)
    + Body

            {
                "processInstId": "1333434",
                "vars":{}
            }

    + Attributes (object)

        + activeProcess (boolean) - 还在进行中的流程
        + canceledProcess (boolean) - 已经被取消的的流程
        + createBy (string) - 由谁创建的流程
        + createdAfter (string) - 在某个时间点后创建的流程,格式2017-04-12 01:30:01
        + createdBefore (string) - 在某个时间点前创建的流程,格式2017-04-12 01:30:01
        + existSubProcess (boolean) - 是否存在子流程
        + finished (boolean) - 已经完成的流程
        + finishedAfter (string) - 某个时间后完成的流程
        + finishedBefore (string) - 某个时间前完成的流程
        + ids (string) - 流程id列表
        + orderByCreateTimeASC (boolean) - 某个时间后完成的流程
        + unfinished (boolean) - 还没完成的流程
        + pageNo (number, required) - int 页码
        + pageCount (number, required) - int 分页大小

+ Response 200 (application/json)
    + Body 
        
            {
             "head":{
                "errorCode":"0",
                "errorMsg":"success"
             }
             "body":{
                "data": [ {
                    "controlState": "active",
                    "createTime": "2017-01-12 12:01:13",
                    "createUser": "zhangsan",
                    "title": "请假流程 2.0",
                    "end": false,
                    "processInstId":"73cdb3fa-5f56-4706-b522-7d2bf5118cff",
                    }
                ]
             }
            }
            
    + Attributes (object)

        + vars (object) - 流程变量             

### 查询任务 [POST /inner/taskService/query]

查询我的代办,这个api的返回值可能不够, 润乔你自己琢磨下在讨论

+ Request (application/json)
    + Body

            {
                "uid": "lisi",
                "processInstId": "73cdb3fa-5f56-4706-b522-7d2bf5118cff",
                "pageNo": 1,
                "pageCount": 100
            }

    + Attributes (object)

        + uid (string) - 任务(节点)执行人
        + processInstId (string) - 流程实例id, 查出该流程实例下的所有任务节点信息
        + beginAfter    (string) - 在此时间之后的任务
        + activieTask    (boolean) - 这个任务是否还没完成
        + beginBefore    (string) - 在此时间之前的任务
        + pageNo (number, required) - int 分页的页码,>1
        + pageCount (number, required) - int 分页大小,大于0
        
+ Response 200 (application/json)
    + Body 
        
            {
             "head":{
                "errorCode":"0",
                "errorMsg":"success"
             }
             "body":{
                 "processInstId": "proc_xkdkfksdsk",
                 "beginTime": "2017-05-03 13:01:30",
                 "controlState":"activi",
                 "taskId":"t_kkkksk134",
                 "title": "记账代帐(企业确认)"
                }
            }
           
    + Attributes (object)

        + processInstId (string) - 流程实例id
        + createTime (string) - 任务创建的时间
        + controlState (string) - 任务状态
        + taskId (string) - 任务id
        + title (string) - 任务标题

        
### 完成我的任务 [POST /inner/taskService/complate]

完成我的代办任务, 让流程往下走

+ Request (application/json)
    + Body

            {
                "executor": "lisi",
                "taskId": "sssskkkkk",
                "vars": {}         
            }

    + Attributes (object)

        + executor (string, required) - 任务(节点)执行人
        + taskId (number, required) - 任务id
        + vars (object) - 流程变量
        
+ Response 200 (application/json)
    + Body 
        
            "head":{
                "errorCode":"0",
                "errorMsg":"success"
            }


### 回退/打回 [POST /inner/taskService/rollback]

审批不通过,任务打回

+ Request (application/json)
    + Body

            {
                "uid": "lisi",
                "taskId": "sssskkkkk",
                "reason": "回退的原因"
                "vars":{}
            }

    + Attributes (object)

        + uid (string, required) - 任务(节点)执行人
        + taskId (number, required) - 任务id
        + reason (string) - 回退原因
        + vars (object) - 流程变量
        
+ Response 200 (application/json)
    + Body 
        
            "head":{
                "errorCode":"0",
                "errorMsg":"success"
            }

### 改签 [POST /inner/taskService/delegate]

改签

+ Request (application/json)
    + Body

            {
                "applicantUser": "lisi",
                "delegateUser": "sssskkkkk",
                "reason": "委托的原因"
                "taskId": "akkdkkdkdk"
            }

    + Attributes (object)

        + applicantUser (string, required) - 委托人
        + delegateUser (string, required) -  受委托人
        + reason (string) - 委托的原因
        + taskId (string) - 任务id
        
+ Response 200 (application/json)
    + Body 
        
            "head":{
                "errorCode":"0",
                "errorMsg":"success"
            }

### 拿到内置表单的url [POST /inner/formService/form-url]

使用流程设计器的时候,绑定在任务节点的表单url

+ Request (application/json)

    + Body

            {
                "sid": "token---skdkfkskd",
                "taskId": "sssskkkkk"
            }

    + Attributes (object)

        + sid (string, required) - 登录时候获取到的token
        + taskId (number, required) - 任务id

+ Response 200 (application/json)

    + Body 
    
            {
            "head":{
                "errorCode":"0",
                "errorMsg":"success"
            },
            "body":{
                "url":"http://xxxxx.com?sid=kkkkk"
            }
            }


### 流程相关的表单变量 [POST /inner/formService/form-vars]

使用流程设计器的时候,绑定在任务节点的表单url

+ Request (application/json)

    + Body

            {
                "sid": "token---skdkfkskd",
                "processInstId": "sssskkkkk"
            }

    + Attributes (object)

        + sid (string, required) - 登录时候获取到的token
        + processInstId (number, required) - 流程实例id

+ Response 200 (application/json)

    + Body 
    
            {
            "head":{
                "errorCode":"0",
                "errorMsg":"success"
            },
            "body":{
                "url":"http://xxxxx.com?sid=kkkkk"
            }
            }    
    
# Data Structures

## ProcParams (object)
+ processDefId (string, required) - 流程定义id
+ title (string, required) - 流程标题
+ uid (string, required) - 流程启动人
+ vars (object) - 流程定义变量
