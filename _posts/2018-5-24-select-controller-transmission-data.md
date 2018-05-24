---
layout:     post
title:      "HTML select标签向后端传值"
date:       2018-5-24 22:32:09
author:     "dxc"
header-img: "img/post-bg-dayanta.jpg"
tags:
    - SSM
    - 前端
---

# HTML select标签向后端传值 #
哎……它竟然能直接传……

## HTML代码 ##
```html
<form action="/op/selectAlgorithm/result" method="post">
    <div class="register-box">
        <label>选择调度算法
            <select name="selected">
                <option>顺序调度</option>
                <option>优先级调度</option>
                <option>资源少优先调度</option>
            </select>
        </label>
    </div>
    <div>
        <input class="submit_btn" type="submit" value="确认">
    </div>
</form>
```

## Java代码 ##
```java
@Controller
@RequestMapping("/op")
public class OpLogController{
    @RequestMapping(value = "/selectAlgorithm/result",method = RequestMethod.POST)
        public String selectAlgorithm( @RequestParam(value = "selected") String al,Model model){
            System.out.println("选择的算法为"+al);
            String alEn;
            if(al.equals("顺序调度")){
                alEn = "orderSched";
            }else if(al.equals("优先级调度")){
                alEn = "prioritySched";
            }else{
                alEn = "resShortSched";
            }
    
            //写入oplog
            opLogService.addOpLogAuto("selectAlgorithm","",alEn);
    
            //回显
            model.addAttribute("algorithm",al);
            return "selectAlgorithmResult";
        }
}
```

通过以下参数来接收：
```java
@RequestParam(value = "selected") String al
```



