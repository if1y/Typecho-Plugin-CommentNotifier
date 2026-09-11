# Typecho-Plugin-CommentNotifier

#### 项目介绍

- Typecho 博客评论邮件提醒，修改自 https://github.com/jrotty/CommentNotifier
- 使用 `fastcgi_finish_request` 的进程内异步发信方案替换 `requestService`，降低因绕路公网导致失败的可能性
- 支持异步回调，减小对博客评论提交速度的影响
- 支持邮件模板
- 支持 SMTP、阿里云邮件推送、API 三种发信方式
- 支持接入邮件密码找回

#### 安装教程

- 下载后将压缩包解压到 `/usr/plugins` 目录
- 文件夹名改为`CommentNotifier`
- 登录管理后台，激活插件
- 配置插件 填写SMTP参数/阿里云邮箱推送参数
- 支持显示大部分主题的评论表情

### 插件升级

小版本升级直接覆盖就行，大版本升级时需要禁用删除旧版本的文件，然后传新的上去！（如果直接覆盖升级了，就禁用重启下）。

⚠️ 从原版换过来同样需要先禁用再启用，然后重新配置。

### 表情回调函数

https://github.com/jrotty 的主题填写 `parseBiaoQing`
https://github.com/MoXiaoXi233/PureSuck-theme 填写 `parseOwOcodes`

同时 `img` 标签的 `class="biaoqing"` 会被插件替换成内置的样式，宽度会被限制为 30px，如果您有多个 `class` 请这样写 `class="biaoqing otherclass"` 请保证 `biaoqing` 处于 `class` 的最前面

#### 软件架构

- `typecho`版本为`1.2.0`及以上
- `php: >=7.2.0`
- 如果启用SMTP加密模式`PHP`需要打开`openssl`扩展
- 邮件服务基于[`PHPMailer`](https://github.com/PHPMailer/PHPMailer/ )

#### 发信逻辑

文章收到新评论后，如果评论有父级，则发提醒给父级评论，否则发给提醒给文章作者；
如果文章作者邮箱为空，则发提醒给站长邮箱（需要在插件设置里设置）；

如果是待审核的评论则提提醒给站长邮箱，等站长在后台审核后再发提醒给评论的父级评论；
如果没有父级评论则发给文章作者；

同时自己评论自己文章，自己回复自己的情况默认不发邮件提醒。

### 邮件模板

在**控制台**→**评论邮件模板**里可以切换以及编辑模板

`template`文件夹里存放的就是邮件发信模板，大家可以参考内置的几个模板来写属于自己的邮件模板，当然也可以在后台直接修改默认模板来达到邮件美化的作用！