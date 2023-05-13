---
title: HTML学习笔记
date: 2022/10/18
updated: 2022/10/18
cover: https://images.alphacoders.com/126/thumbbig-1265716.webp
top_img: 
description: 学习笔记啦~
swiper_index: 17 #置顶轮播图顺序，非负整数，数字越大越靠前
categories: HTML
---

# 【LittleXi】HTML学习笔记

data:2022/10/18 学习至多行文本框控件
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style type="text\css">
        h1.style{border-width: 1;border: solid;
        color: red;text-align: center;
        }
    </style>
    <title>Document</title>


</head>

<!-- <frame row="300,*" col="*">
    <frame src="https://www.bilibili.com/"></frame>
    <frame src="https://www.bilibili.com/video/BV19d4y1z7R8/?spm_id_from=333.1007.tianma.1-2-2.click"></frame>
</frame> -->
<body>
    <a target="_blank"title="hhh"href="https://blog.csdn.net/qq_68591679?spm=1000.2115.3001.5343">my blog</a>
    <a href="photo.png" download="photo.png">下载帅照</a>
    <a href="#st">毛连接</a>






    <h1 class="./style">
        使用了c<sup>22</sup>样式!
    </h1>
    <h1 >
        没有使<q>用了c</q>cs样式!
    </h1>
    <hr>
    <time>

    </time>
    <p>
        C:\U<abbr>s\夕</abbr>ser\Des<mark>ktop\各种文件夹</mark>\图片\捕111获.PNG
        <br>
        <code>
            var num;
            class Solution:
                def isPalindrome(self, s: str) -> bool:
                    l=[]
                    for c in s:
                        if c>='a' and c<='z':
                            l.append(c)
                        if c>='A' and c<='Z':
                            l.append(c)
                    return l==l.__reversed__()
        </code>
    </p>
    <p>
             C:\Users\夕\Desktop\各种<br>文件夹\图片\捕111获.PNGC:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNGC:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNGC:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNG
    </p>
    <strong id="st">加粗</strong>
    <br>
    <u>下划线</u>
    <p></p>
    <em>倾斜</em> 
    <del>删除线</del>
    <img src="photo.png"
    
    alt="我是替换文本"
    title="这是title文字，鼠标悬浮时显示"
    width="100"
    height="100"
    >
    <img src="photo.png" 
    width="100px"   
    alt="false"target="_blank"text-align="center"> 
    <!-- <audio src="./y860.mp3" controls loop autoplay></audio> -->
    <video width="100px" src="https://cn-nmghhht-cm-01-10.bilivideo.com/upgcxcode/02/55/377685502/377685502_nb2-1-16.mp4?e=ig8euxZM2rNcNbRVhwdVhwdlhWdVhwdVhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1666103303&gen=playurlv2&os=bcache&oi=2029896750&trid=0000d80ef855a17341aeb8cc0bf7ccb7a1e3T&mid=524432272&platform=html5&upsig=b4a47114239edab3c1fb7bd02672f9e9&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,mid,platform&cdnid=6672&bvc=vod&nettype=0&bw=43888&orderid=0,1&logo=80000000" controls >

    </video>
    <a target="_blank" href="https://blog.csdn.net/qq_68591679?spm=1000.2115.3001.5343">
        <img src="photo.png" 
        width="100px"   
        alt="false"target="_blank"text-align="center"> 
    </a>
    <ol >
        <!-- <li><a href="https://blog.csdn.net/qq_68591679?spm=1000.2115.3001.5343"
        >hello blog</a></li> -->
        <li>hello lym</li>
        <li>hello litllexi</li>
    </ol>
    <dl>
        <dt>lym<dd>xiaoxi</dd></dt>
        <dt>lym<dd>xiaoxi</dd></dt>
        <dt>lym<dd>xiaoxi</dd></dt>
    </dl>
    <p>
        C:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNG
    </p>
    <p>
             C:\Users\夕\Desktop\各种<br>文件夹\图片\捕111获.PNGC:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNGC:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNGC:\Users\夕\Desktop\各种文件夹\图片\捕111获.PNG
    </p>
    <hr>
    <table border="3" align="center" id="tab" style="border-color: blue;">
        <caption>小学生C++课表</caption>
        <tr ><td colspan="6" align="center">合并课表</td></tr>
        <colgroup span="1" style="width: 80px;background-color: greenyellow;"></colgroup>
        <tr>
            <td>monday</td>
            <td>tusday</td>
            <td>Wednesday</td>
            <td>Thursday</td>
            <td>friday</td>
            <td>sunday</td>
        </tr>
        <colgroup span="5"style="width: 120px;">
            <col style="background-color: rgb(250,100,0);">
            <col style="background-color: rgb(150,150,0);">
            <col style="background-color: rgb(250,180,0);">
            <col style="background-color: hwb(133 0% 2%);">
            <col style="background-color: rgb(158, 250, 0);">
 
        </colgroup>
            <tr>
                <td rowspan="4" align="center">上午</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
            <tr>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
            <tr>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
            <tr>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
            <tr>
                <td rowspan="3" align="center">下午</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
            <tr>
               
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
            <tr>               
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
                <td>&nbsp</td>
            </tr>
        </tr>
    </table>
    <script>
        var mid=document.querySelector("tab")
        
    </script>
    <input type="text" size="50" maxlength="30">
    <br>
    <input type="text" name="myname">
    <br>
    <input type="password" name="psw">
    <br>
    <input type="submit" name="1" value="yes" >
    <br>
    <input type="reset" name="2" value="no">
    <br>
    <input type="button" name="3" value="btn">

    <h2 style="text-align: center;">用户登录界面</h2>
    <hr>
    <br><br>
    <form>
        <p style="text-align: center;">
            用户名:&nbsp&nbsp&nbsp&nbsp<input type="text"name="myname">
        </p>
        <p style="text-align: center;">
            keyword&nbsp&nbsp&nbsp<input type="password"name="psw" maxlength="11">
        </p>
        <p style="text-align: center;">
            &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp
            <input type="submit"name="su">
            &nbsp&nbsp<input type="reset" name="re">         
        </p>
        
    </form>
    <br><br><br><br>
    <h1 style="text-align: center;">个人信息统计</h1>
    <hr><br>
    <table align="center" style="color: rgb(58, 197, 65);">
        <form>
            <tr>
                <td>用&nbsp;户&nbsp;名&nbsp;&nbsp;</td>
                <td ><input type="text"name="username"></td>
            </tr>
            <tr>
                <td>性&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;别</td>
                <td><input type="radio" name="mysex" value="boy">男</td>
                <td><input type="radio" name="mysex" value="girl">女</td>
                
            </tr>
            <tr>
                <td>
                    <div>选择你的城市</div>
                </td>
                <td>
                    <p style="text-align: center;">                       
                        <select style="width: 200px;">
                            <option value="">北京</option>
                            <option value="">上海</option>
                            <option value="">天津</option>
                        </select>
                    </p>
                </td>

            </tr>
                
            <tr>
                <td>爱&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;好</td>
                <td>
                    <input type="text" list="lis">
                    <datalist id="lis">
                        <option value="cpp"></option>
                        <option value="java"></option>
                        <option value="python"></option>
                        <option value="js"></option>
                    </datalist>
                </td>


            </tr>
            <tr>
                <td>爱&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;好</td>
                <td><input type="checkbox">cpp
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <input type="checkbox">python
                    <br>
                    <input type="checkbox">js
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <input type="checkbox">java
                </td>
            </tr>
            <tr>
                <td>电子邮箱拿来把你:&nbsp;</td>
                <td>
                    <input type="email" required>
                </td>
            </tr>
            <tr>
                <td></td>
                <td >
                    <p>
                        <input type="image" src="photo.png" width="200px" height="30px">
                    </p>
                </td>
            </tr>
            <tr>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <input type="submit">                
                </td>
                <td>
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    <input type="reset">
                </td>
            </tr>
        </form>
    </table>

    <h1 style="text-align: center;">网址</h1>
    <hr>
    <form action="">
        <p  style="text-align: center;">网站的地址：<input type="url"></p>
        <p style="text-align: center;"><input width="20" type="image" src="photo.png"></p>
    </form>

    <form >
        <p style="text-align: center;"><input type="number" min="1" max="100" step="5" value="10"></p>
        <p style="text-align: center;">
            请设置原石多少<input type="range"min="1" max="100" step="5" value="10">
        </p>
    </form>
    <form>
        <p  style="text-align: center;">
            <input type="date">
            <br><br>
            <input type="time">
            <br><br>
            <input type="month">
            <br><br>
            <input type="datetime-local">
        </p>
    </form> 

    <form>
        <p style="text-align: center;">
            <input type="search" placeholder="关键字">
            <br><br>
            <input type="color">
            <br><br>
            <input type="file">
        </p>
    </form>

    <form >
        <p style="text-align: center;">
            <textarea rows="10" cols="30" placeholder="请随意发言"></textarea>
        </p>
    </form>

    <form oninput="o.value=parseInt(i1.value)^parseInt(i2.value)">
        <p style="text-align: center;">
            <input type="number"  name="i1" id="a">X
            <input type="number" name="i2" id="b">=
            <output name="o" for="a b"></output>
        </p>
    </form>

    <form >
        <p style="text-align: center;">
            <progress value="50" max="100">
            </progress>
            <br>
            <meter max="45" min="5" value="16" low="10" high="37" optimum="25">

            </meter>
        </p>
    </form>
    <!---->
    <form >
        <p style="text-align: center;">
            <label>啦啦啦</label>
            <select>
                <optgroup label="hh啊啊啊啊啊h">
                    <option value="哈哈哈">哈哈哈</option>
                    <option value="笑死">😄</option>
                    <option value="萨达">蛤😄</option>
                </optgroup>
                <optgroup label="www">
                    <option value="哭泣">/(ㄒoㄒ)/~~</option>
                    <option value="qwq">qwq</option>
                    <option value="对我">QWQ</option>
                </optgroup>
            </select>
        </p>
    </form>
</body>

</html>
```