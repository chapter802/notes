---
title: JavaScript Snippets
tags: JavaScript
---
### 说明

总结收集了一些开发中常用的方法，如有错误纰漏还望大家多多指正。

### 日期类

#### 当前日期向后推N天

  原生javascript

   ``` Javascript
    function addDate(date,days){
        var d=new Date(date);
        d.setDate(d.getDate()+days);
        var month=d.getMonth()+1;
        var day = d.getDate();
        if(month<10){
            month = "0"+month;
        }
        if(day<10){
            day = "0"+day;
        }
        var val = d.getFullYear()+""+month+""+day;
        return val;
    }
        console.log(addDate("2017/7/15",90)); // 20171013

   ```

#### 当前日期后推N个月

   原生javascript

   ```Javascript
    //传递一个参数，参数值为向后推迟几个月
    function getStartDate(monthNum) {
        var newDate = new Date();
        var startY = newDate.getFullYear();
        var startM = newDate.getMonth() + monthNum;
        var startD = newDate.getDate();
        // 日期賦值
        var mydate = new Date(startY, startM, startD);
        var strYear = mydate.getFullYear();
        //顯示的月份要加一，因為月份是從0開始的
        var strMonth = mydate.getMonth() + 1;
        var strDate = mydate.getDate();
        if (strMonth.toString().length == 1)
            strMonth = "0" + strMonth;
        if (strDate.toString().length == 1)
            strDate = "0" + strDate;
        var strStartDate = strYear + "-" + strMonth + "-" + strDate;
        return strStartDate;
    }
    ```

<!-- more -->
  
#### 根据出发日期和结束日期计算入住天数

   原生javascript
  
    ```Javascript
    //传递两个参数，start:起始日期 end：结束日期
    function duringDays(start, end) {

        var date = new Date(end);
        var y = date.getFullYear();
        var m = date.getMonth();
        var d = date.getDate();
    
        var endDate = new Date(start);
        var y1 = endDate.getFullYear();
        var m1 = endDate.getMonth();
        var d1 = endDate.getDate();
        var endTime = new Date(parseInt(y1), parseInt(m1) - 1, parseInt(d1));
        var nowTime = new Date(parseInt(y), parseInt(m) - 1, parseInt(d));
        var day = (Number(nowTime) - Number(endTime)) / (1000 * 60 * 60 * 24);
    
        return day;
    }

    ```
  
#### 通用计算时间差函数

  原生javascript

   ```Javascript
      function duringTime(start) {
    
        var start_ms = Date.parse(new Date(start.replace(/-/g, "/")));        // starttime 为开始时间 starttime_ms为毫秒级时间
        var end_ms = Date.parse(new Date());
        var days = Math.floor((end_ms - start_ms) / (24 * 3600 * 1000));     //天数差
        var gap = (end_ms - start_ms) % (24 * 3600 * 1000);                  //当前时间-开始时间，时间差的毫秒数%(24*3600*1000)
        var hours = Math.floor(gap / (3600 * 1000));                         //小时差
    
        var gap2 = gap % (3600 * 1000);                                    //计算小时数后剩余的毫秒数
        var minutes = Math.floor(gap2 / (60 * 1000));                        //分钟差
        var gap3 = gap2 % (60 * 1000);                                     //计算分钟数后剩余的毫秒数
    
        //var seconds = Math.floor(gap3/(1000));                         //秒数差
    
        var result = function (result) {
            if (hours === 0) {
                result = minutes + '分钟前';
            } else if (days === 0) {
                result = hours + '个小时前';
            } else if (days > 0) {
                result = days + '天前';
            }
            return result;
        };
        return result();
    }
   ```
   
#### 通用日期格式化函数

  原生javascript
    
   ```Javascript
    //格式化完整日期包含星期
function fullDate(date) {
    var day = new Date(date).Format("yyyy年MM月dd日");       //日期格式化为yyyy年MM月dd日
    var week = new Date(date).getDay();
    return day + '&nbsp;&nbsp;' + '星期' + switchWeek (week);
}

//格式化日期HH:mm
function hourAndMinute(date) {
    var cTime = new Date(date).Format("hh:mm");
    return cTime;
}

Date.prototype.Format = function (fmt) { //author: meizz
    var o = {
        "M+": this.getMonth() + 1, //月份
        "d+": this.getDate(), //日
        "h+": this.getHours(), //小时
        "m+": this.getMinutes(), //分
        "s+": this.getSeconds(), //秒
        "q+": Math.floor((this.getMonth() + 3) / 3), //季度
        "S": this.getMilliseconds() //毫秒
    };
    if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
    for (var k in o)
        if (new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    return fmt;
};

function switchWeek (week) {
    var weekday;
    switch (week) {
        case 1:
            weekday = '一'; break;
        case 2:
            weekday = '二'; break;
        case 3:
            weekday = '三'; break;
        case 4:
            weekday = '四'; break;
        case 5:
            weekday = '五'; break;
        case 6:
            weekday = '六'; break;
        default:
            weekday = '日'; break;
    }
    return weekday;
}

```

### 获取浏览器地址参数

原生javascript

```Javascript
    function getParameters() {
      var searchString = window.location.search.substring(1),
          params = searchString.split("&"),
          hash = {};
    
      if (searchString == "") return {};
      for (var i = 0; i < params.length; i++) {
        var val = params[i].split("=");
        hash[unescape(val[0])] = unescape(val[1]);
      }
      return hash;
    }    //返回一个json对象
```

### rem px转换

原生javascript

```Javascript
    function convertRemToPixels(rem) {    
        return rem * parseFloat(getComputedStyle(document.documentElement).fontSize);
    }

```

### Cookie

原生javascript

```Javascript
    //创建cookie
    function createCookie(name, value, days) {
        var expires = "";
        if (days) {
            var date = new Date();
            date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
            expires = "; expires=" + date.toUTCString();
        }
        document.cookie = name + "=" + value + expires + "; path=/";
    }
    //读取Cookie
    function readCookie(name) {
        var nameEQ = name + "=";
        var ca = document.cookie.split(';');
        for (var i = 0; i < ca.length; i++) {
            var c = ca[i];
            while (c.charAt(0) == ' ') c = c.substring(1, c.length);
            if (c.indexOf(nameEQ) == 0) return c.substring(nameEQ.length, c.length);
        }
        return null;
    }
    //删除Cookie
    function eraseCookie(name) {
        createCookie(name, "", -1);
    }
```

### 输入框只能输入数字

* HTML 5 (does not require JavaScript, and also does not behave in standard way in many modern browsers.)
  `<input type=“number">`
* jQuery

    ```Javascript
    $(document).ready(function() {
        $("#txtboxToFilter").keydown(function (e) {
            // Allow: backspace, delete, tab, escape, enter and .
            if ($.inArray(e.keyCode, [46, 8, 9, 27, 13, 110, 190]) !== -1 ||
                // Allow: Ctrl/cmd+A
                (e.keyCode == 65 && (e.ctrlKey === true || e.metaKey === true)) ||
                // Allow: Ctrl/cmd+C
                (e.keyCode == 67 && (e.ctrlKey === true || e.metaKey === true)) ||
                // Allow: Ctrl/cmd+X
                (e.keyCode == 88 && (e.ctrlKey === true || e.metaKey === true)) ||
                // Allow: home, end, left, right
                (e.keyCode >= 35 && e.keyCode <= 39)) {
                // let it happen, don't do anything
                return;
            }
            // Ensure that it is a number and stop the keypress
            if ((e.shiftKey || (e.keyCode < 48 || e.keyCode > 57)) && (e.keyCode < 96 || e.keyCode > 105)) {
                e.preventDefault();
            }
        });
    });
    ```
    
### 事件注册
    
```Javascript
    var EventUtil = {
        addEvent: function(element, type, handler) {
            if(element.addEventListener) { //DOM2级
                element.addEventListener(type, handler, false);
            }else if(element.attachEvent) {  //IE
                element.attachEvent("on"+ type, handler);
            }else {
                element["on" + type] = handler;
            }
        },
        removeEvent: function(element, type, handler) {
            if(element.removeEventListener) { //DOM2级
                element.removeEventListener(type, handler, false);
            }else if(element.detachEvent) {  //IE
                element.detachEvent("on"+ type, handler);
            }else {
                element["on" + type] = null;
            }
        },
        stopPropagation: function(ev) {
            if(ev.stopPropagation) {
                ev.stopPropagation();
            }else {
                ev.cancelBubble = true;
            }
        },
        preventDefault: function(ev) {
            if(ev.preventDefault) {
                ev.preventDefaule();
            }else {
                ev.returnValue = false;
            }
        },
        getTarget: function(ev) {
            return event.target || event.srcElement;
        },
        getEvent: function(e) {
            var ev = e || window.event;
            if(!ev) {
                var c = this.getEvent.caller;
                while(c) {
                    ev = c.arguments[0];
                    if(ev && Event == ev.constructor) {
                        break;
                    }
                    c = c.caller;
                }
            }
            return ev;
        }
    };

```


