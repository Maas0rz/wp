#### 【md5】

##### <<绕过

&emsp;&emsp;参考[这篇文章](https://github.com/Sunset-airplane/Learn/blob/main/%E3%80%90PHP%E3%80%91%E7%B1%BB%E5%9E%8B%E6%AF%94%E8%BE%83.md)，当字符串为 `^[0-9]+e[0-9]` 模式时，将试图把字符串转换为用科学计数法表示的数，设为 x ，最终变成 "x" 。但如果不是真正的科学计数法表示的数，强制转换将抛出错误，所以 PHP 索性将其当作不会抛出错误的（即相对而言最正确的）东西——字符串。

&emsp;&emsp;所以，如果两字符串的 md5 值都是 `^[0-9]+e[0-9]` 模式，且 e 后全部是数字，则可绕过。

&emsp;&emsp;以下字符串的 md5 值均符合上述要求：

- QNKCDZO
- 240610708
- s878926199a
- s155964671a

##### <<例题

###### [SWPUCTF 2021 新生赛]easy_md5

[题目链接](https://www.ctfer.vip/problem/386)

题解：name=QNKCDZO ，password=240610708 。

#### 【HTTP】

##### <<例题

&emsp;&emsp;过于普通的题，比如 xff、Referer、GET/POST 传参等，此处忽略。

###### [NCTF 2018]签到题

[题目链接](https://www.ctfer.vip/problem/958)

题解：

&emsp;&emsp;F12 查看 Network 。不过这题很坑啊，flag 不在 secret.php 的请求包里，而在 302 状态码请求 / 的那个包里，同时得在 url 还没转圈转到 xxx/secret.php 时就 F12，不然看不到这个包。

&emsp;&emsp;实际上从访问 url 时转了那么久的圈就应该要察觉：可能有猫腻！

#### 【游戏】

##### <<绕过

- 玩通关
- 正常解法

##### <<例题

###### [HNCTF 2022 Week1]2048

[题目链接](https://www.ctfer.vip/problem/2898)

- 源码大致内容：

  ```html
  <!DOCTYPE HTML >
  <html >
  	<head>
  		<title>2048</title>
  		<meta charset="utf-8"/>
  		<meta name="author" content="石亚平" />
  		<style>
  			//略
  			<script src="md5.js"></script>
  		</style>
  		<script src="2048.js"></script>
  	</head>
  	<body>
  		<p>Score:<span id="score"></span></p>
  		<div id="gridPane1">
  			//略
  		</div>
  		<!--Game Over界面-->
  		<div id="gameover">
  			<!--灰色半透明背景-->
  			<div></div>
  			<!--前景小窗-->
  			<p>Game Over!<br/>
  				Score:<span id="finalScore"></span><br/>
  				<a class="button" id="restart" onclick="game.start()">Try again!</a>
  			</p>
  		</div>
  
  
    
  	</body>
  </html>
  ```

  发现没有，它没有提交 score 的部分，并且你玩死了之后，像这样：
  
  ![](picture/1.png)
  
  点击 Try again! 后页面不会刷新，也就没有包，说明 score 压根不会上传给服务端。也就是说你再怎么玩，20000 分了它也不给你 flag 。
  
- 注意到源码里有两个 .js 文件，其中 2048.js 是可访问链接，访问它，注意到：

  ```js
  if (this.score > 20000)
  		{
  		   //var md5util= require("md5.js"); //å†™æ–‡ä»¶è·¯å¾„å³å¯
              alert(String.fromCharCode(24685,21916,33,102,108,97,103,123,53,51,49,54,48,99,56,56,56,101,50,53,99,51,102,56,50,56,98,50,51,101,51,49,54,97,55,97,101,48,56,51,125));
  		}
  ```

  很明显这是给 flag 的地方，fromCharCode() 是解 Unicode 编码的函数，运行可得 flag 。

###### [Hackergame 2022]签到

- 别想了，手速没那么块，不可能成功签出 2022 的。
- 注意到点击“提交”时，GET 方式传入了 result 变量，将其值改为 2022 ，访问既得 flag 。