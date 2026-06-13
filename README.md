
  <h1 align="center">🏋️ 智能健身管理平台 — FitHub</h1>
  <p align="center">
    <img src="https://img.shields.io/badge/Vue-3.4-4FC08D?logo=vuedotjs" alt="Vue" />
    <img src="https://img.shields.io/badge/TypeScript-5.2-3178C6?logo=typescript" alt="TypeScript" />
    <img src="https://img.shields.io/badge/Spring_Boot-2.4-6DB33F?logo=springboot" alt="Spring Boot" />
    <img src="https://img.shields.io/badge/Dubbo-2.8-FF6F00?logo=apache" alt="Dubbo" />
    <img src="https://img.shields.io/badge/MySQL-5.7-4479A1?logo=mysql" alt="MySQL" />
  </p>
</h1>

一个功能完整的健身房后台管理系统，涵盖会员管理、课程运营、商品销售、权限控制等核心业务场景，支持 **Spring Boot 单体**与 **Dubbo 微服务**双架构部署。

---

## ✨ 功能模块

| 模块 | 功能 |
|------|------|
| **系统管理** | 员工 / 教练管理、RBAC 角色权限、动态菜单、按钮级权限控制 |
| **会员管理** | 会员 CRUD、会员卡类型配置、购卡记录、余额充值 |
| **课程管理** | 课程发布（富文本）、学员报名、课程订单、退费处理 |
| **器材管理** | 器材录入、库存管理 |
| **商品管理** | 商品 CRUD、销售订单 |
| **失物招领** | 失物登记、认领状态跟踪 |
| **反馈管理** | 会员建议收集与处理 |
| **数据仪表盘** | ECharts 可视化统计总览 |

## 🛠 技术栈

### 前端

- **Vue 3** Composition API + **TypeScript**
- **Element Plus** UI 组件库
- **Pinia** 状态管理 + 持久化
- **Vue Router** 动态路由
- **ECharts** 数据可视化
- **Axios** HTTP 请求封装
- **Vite** 构建工具

### 后端

- **Spring Boot 2.4** 主框架
- **Spring Security** + **JWT** 认证授权
- **MyBatis-Plus** ORM
- **Druid** 数据库连接池
- **MinIO** 对象存储
- **Swagger** API 文档
- **Kaptcha** 图形验证码
- **EasyPoi** Excel 导入导出

### 微服务（演进版）

- **Apache Dubbo** RPC 通信
- **ZooKeeper** 服务注册与发现

## 📁 项目结构

```
gym-pro/
├── data/
│   ├── image/assets/              # 静态图片资源
│   └── sql/yun.sql                # 数据库初始化脚本（14 张表）
├── yun/
│   ├── frontend/gymnasium-project/  # 前端 SPA
│   ├── backend/
│   │   ├── gymnasium-parent-project/   # ★ 单体架构（Spring Boot）
│   │   ├── springboot-dubbo-project/   # Dubbo 集成演示
│   │   └── swagger-demo/               # Swagger 配置演示
│   └── transform/
│       └── gymnasium-parent-project/   # ★ 微服务架构（7 个 Dubbo 服务）
└── application-local.yml.example  # 本地配置模板
```

## 🚀 快速开始

### 环境要求

- **JDK** 1.8+
- **Node.js** 16+
- **MySQL** 5.7+
- **Maven** 3.6+
- **MinIO**（可选，文件上传需要）
- **ZooKeeper**（可选，仅微服务版需要）

### 1. 初始化数据库

```bash
# 创建数据库并导入
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS gymnasium DEFAULT CHARSET utf8mb4;"
mysql -u root -p gymnasium < data/sql/yun.sql
```

### 2. 配置本地环境

```bash
# 复制配置模板
cp application-local.yml.example application-local.yml

# 编辑填入真实密码（此文件已在 .gitignore 中排除）
# DB_PASSWORD / MINIO_ACCESS_KEY / MINIO_SECRET_KEY / JWT_SECRET
```

> 也可以直接设置环境变量：`DB_PASSWORD`、`MINIO_ACCESS_KEY`、`MINIO_SECRET_KEY`、`JWT_SECRET`

### 3. 启动后端（单体版）

```bash
cd yun/backend/gymnasium-parent-project
mvn clean install -DskipTests
cd gymnasium-service-web
mvn spring-boot:run
# 启动在 http://localhost:8089
```

### 4. 启动前端

```bash
cd yun/frontend/gymnasium-project
npm install
npm run dev
# 启动在 http://localhost:8080
```

### 5. 微服务版（可选）

```bash
# 先启动 ZooKeeper，然后依次启动各服务：
cd yun/transform/gymnasium-parent-project

# 服务提供方（按需启动）
mvn spring-boot:run -pl gymnasium-service-course   # 8081
mvn spring-boot:run -pl gymnasium-service-member   # 8083
# ... 其他服务

# 服务消费方（API 网关）
mvn spring-boot:run -pl gymnasium-service-web      # 8089
```

## 🔐 内置账号

| 角色 | 用户名 | 说明 |
|------|--------|------|
| 系统管理员 | `admin` | 所有权限 |
| 员工 | `zhangsan` | 业务操作 |
| 会员 | 卡号登录 | 自助查询 |

> 注：密码均为 BCrypt 加密，首次使用需通过数据库重置或注册

## 🧱 架构演进

| 版本 | 路径 | 架构 |
|------|------|------|
| v1 单体 | `yun/backend/gymnasium-parent-project/` | Spring Boot 单体，单数据库 |
| v2 微服务 | `yun/transform/gymnasium-parent-project/` | Dubbo + ZooKeeper，7 个业务服务 + 1 个网关 |

微服务拆分策略：按业务域垂直拆分——课程、商品、会员、菜单、角色、反馈、用户各为独立服务，共享同一数据库。

## 📄 License

[MIT](LICENSE)

---

<p align="center"><sub>Made with ☕ by jiangtianxi627-boop</sub></p>
