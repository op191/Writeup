**Drush = Drupal Shell**

是 **Drupal 专属命令行工具**，专门在服务器终端里，不用进网页后台，就能**管理 Drupal 网站**：改管理员密码、装插件、清缓存、建用户、执行网站命令。

DC7 靶机里预装了 drush，所以可以直接在服务器命令行**强行重置网站超级管理员 admin 的密码**。

切换但 网站根目录 /var/www/html 才可以使用drush命令

Drupal 8版本以上模块是没有php filter 需要到网络上搜，官网已经下架了该模块下载

添加后台用户

```
drush user-password 用户名 --password 新密码
```

