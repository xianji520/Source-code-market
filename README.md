源码市集v1:
# 源码市集

一套可直接商用的 PHP 源码交易平台：前台展示 + 用户系统 + 余额支付 + 后台管理 + 安装向导，单文件 SQLite 数据库零配置即可跑起来。

## 功能清单

### 前台
- 首页（分类、热门、最新）
- 源码列表（搜索、分类筛选、价格排序）
- 源码详情（封面、Markdown 介绍、演示链接）
- 注册 / 登录 / 退出
- 购物车（加入、调整数量、移除）
- 结算（余额扣款，自动建订单）
- 我的订单（查看、下载、取消）
- 个人中心（资料、修改密码、余额充值申请、流水）

### 后台（`/admin/`）
- 仪表盘（用户数、订单数、销售额、最近动态）
- 源码管理（新增、编辑、上下架、删除）
- 上传源码（ZIP / RAR / 7Z，最大 200MB 可配）
- 分类管理
- 订单管理（标记完成、取消并退款）
- 用户管理（启用/禁用、查看详情、调余额、重置密码）
- 充值审核（通过 / 拒绝）
- 站点设置（站名、ICP、客服邮箱等）

## 环境要求
- PHP 7.2+
- 扩展：`pdo`、`pdo_sqlite` 或 `pdo_mysql`、`mbstring`、`json`、`session`
- 写权限：`data/`、`uploads/`、`config.php`
- 推荐：Apache + mod_rewrite（自带 `.htaccess`）；Nginx 需手动配置

## 安装步骤
1. 把整个目录上传到服务器
2. 浏览器访问 `http://你的域名/install.php`
3. 按向导填写：站点信息、数据库、管理员账号
4. 安装完成会自动跳转到首页
5. 用管理员账号登录 `http://你的域名/admin/login`

默认推荐 SQLite，无需额外配置。如果用 MySQL，先在数据库里建好空库，安装时填入连接信息即可。

## 目录结构
```
网站/
├── install.php          安装向导
├── bootstrap.php        启动入口（所有页面 require）
├── config.example.php   配置模板
├── index.php            官网首页
├── login.php / register.php / logout.php
├── products.php / product.php
├── cart.php / checkout.php / orders.php / download.php
├── profile.php
├── includes/
│   ├── db.php           PDO + 建表
│   ├── auth.php         会话/登录/CSRF
│   └── functions.php    公共函数
├── admin/
│   ├── _top.php         后台框架
│   ├── login.php / logout.php
│   ├── index.php        仪表盘
│   ├── products.php / product_edit.php / upload.php
│   ├── categories.php
│   ├── orders.php
│   ├── users.php / user_edit.php
│   ├── recharges.php    充值审核
│   └── settings.php
├── assets/
│   ├── css/             样式
│   └── img/             图片
├── data/                SQLite 数据库 / 安装锁
└── uploads/
    ├── products/        源码压缩包
    └── covers/          封面图
```

## 重新安装
删除 `data/.installed` 与 `data/store.db`，再访问 `install.php` 即可。

## 安全说明
- 密码使用 `password_hash()`（bcrypt）
- 所有写操作走 PDO 预处理，防止 SQL 注入
- 所有表单带 CSRF token
- 所有输出经过 `htmlspecialchars` 转义
- 安装完成后请勿删除 `data/.installed`，否则可能被覆盖重装
- 配置文件 `config.php` 含数据库密码，请勿放在公开仓库

## 开发者备注
- 单文件 SQLite 适合小型项目（≤10万订单）。如果用户量大，请切换到 MySQL
- 后台默认不限制上传文件大小（200MB），受限于 `php.ini` 的 `upload_max_filesize` 与 `post_max_size`，可能需要调整
- 余额系统是内建的，没有接第三方支付。生产环境接微信/支付宝需要扩展 `checkout.php`
