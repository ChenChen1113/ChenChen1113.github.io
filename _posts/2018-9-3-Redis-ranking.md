---
layout:     post
title:      "SSM架构前端HTML与后台controller互传数据"
date:       2018-5-24 19:45:20
author:     "dxc"
header-img: "img/home-bg-o.jpg"
tags:
    - SSM
    - 前端
---
# SSM架构前端HTML与后台controller互传数据 #

本篇记录的是在HTML标签中直接与后台互传数据的方法。

## 1、HTML传数据到controller ##
### HTML代码 ###
在查询指定任务时，输入一个任务编号，点击查询，代码如下：
```html
<form action="/job/list/result" method="post">
    <div class="register-box">
        <label>任务编号
            <input maxlength="20" type="text" name="findJobId" id="findJobId" placeholder="请输入任务编号"/>
        </label>
    </div>
    <div>
        <input class="submit_btn" type="submit" value="查询">
    </div>
</form>
```
简化一下：
```html
<form action="/job/list/result" method="post">
    <label>任务编号
        <input type="text" name="findJobId" placeholder="请输入任务编号"/>
    </label>
    <input type="submit" value="查询">
</form>
```

### Java代码 ###
```java
@Controller
@RequestMapping("/job")
public class JobController{

@RequestMapping(value = "/list/result",method = RequestMethod.POST)
    public String showJobByID(@RequestParam(value = "findJobId")String jobId,Model model){
        if(jobService.getJobNumByID(jobId)!=0){
            //写入oplog
            opLogService.addOpLogAuto("showJob",jobId,"");
            //传回信息
            List<Job> list = jobService.findJobById(jobId);
            model.addAttribute("jobList",list);
            return "jobListResult";
        }
        else{
            return "jobList";
        }
    }
    
}
```

简化一下：
```Java
@Controller
@RequestMapping("/job")
public class JobController{

    @RequestMapping(value = "/list/result",method = RequestMethod.POST)
        public String showJobByID(@RequestParam(value = "findJobId")String jobId,Model model){
            List<Job> list = jobService.findJobById(jobId);
            model.addAttribute("jobList",list);
            return "jobListResult";
        }
    
}
```
说明：HTML与controller通过input标签中的name传数据，而不是id。
```Java
public String showJobByID(@RequestParam(value = "findJobId")String jobId,Model model)
```
在后台Java代码中，利用方法的参数来接收前端传来的数据，将findJobId中的内容赋给jobId，默认为String类型，在方法内部即可使用jobId。


若一次表单提交中有多个值需要传递，Java接收参数如下：
```java
@RequestMapping(value = "/add/result",method = RequestMethod.POST)
    public String addJob(@RequestParam(value = "job_id")String job_id,
                         @RequestParam(value = "job_type")String job_type,
                         @RequestParam(value = "job_name")String job_name,
                         @RequestParam(value = "start_time")String start_time,
                         @RequestParam(value = "end_time")String end_time,
                         @RequestParam(value = "exe_cycle")String exe_cycle,
                         @RequestParam(value = "priority")String priority,
                         @RequestParam(value = "res")String res,
                         Model model){
                            //实现方法
                         }
```


## 2、controller传数据到HTML---传递多个值 ##
### java代码 ###
将在数据库中查询到的job列表放入jobList中，在前端对jobList进行读取显示。
返回HTML页面的文件名。
```java
@Controller
@RequestMapping("/job")
public class JobController{

    @RequestMapping("/list")
        public String showAllJob( Model model){
            List<Job> list = jobService.findAllJob();
            model.addAttribute("jobList",list);
            return "jobList";
        }
        
}
```
### HTML代码 ###
```html
<table>
    <thead>
        <tr>
            <th>任务编号</th>
            <th>任务名称</th>
            <th>任务类型</th>
            <th>最初开始时间</th>
            <th>最初结束时间</th>
            <th>循环周期</th>
            <th>任务当前状态</th>
            <th>优先级</th>
            <th>所占资源</th>
            <th>已执行次数</th>
        </tr>
    </thead>
    <tbody>
        <tr th:each="j:${jobList}">
            <td th:text="${j.getJob_id()}"></td>
            <td th:text="${j.getJob_name()}"></td>
            <td th:text="${j.getJob_type()}"></td>
            <td th:text="${j.getStart_time()}"></td>
            <td th:text="${j.getEnd_time()}"></td>
            <td th:text="${j.getExe_cycle()}"></td>
            <td th:text="${j.getJob_state()}"></td>
            <td th:text="${j.getPriority()}"></td>
            <td th:text="${j.getRes()}"></td>
            <td th:text="${j.getExe_times()}"></td>
        </tr>
    </tbody>
</table>
```
使用了spring官方的thymeleaf框架。需要在开头：
```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```
## 3、controller传数据到HTML---传递单个值 ##
### Java代码 ###
controller中的代码是一样的，利用addAttribute。
```java
model.addAttribute("algorithm",al);
```
### HTML代码 ###
```html
<div class="register-box">
    <p>选择算法成功！当前算法： </p>
    <p th:text="${algorithm}"></p>
</div>
```
## 4、备注 ##
这是在前端基础≈0时的使用方法，简单可以解决问题，但是不是最优的方法……