# 苍穹外卖系统

> 苍穹外卖是一个基于Golang开发的餐饮外卖管理系统，采用前后端分离架构，提供了完整的餐饮企业内部管理和用户点餐功能。

## 项目介绍

苍穹外卖系统是一个完整的餐饮外卖解决方案，包含管理端和用户端两大模块。系统采用Golang语言开发后端服务，使用Gin框架构建Web API，GORM作为ORM框架操作数据库，并集成了Redis缓存、JWT认证等技术。

本项目提供了一个初始的项目架构和思想，适合想要学习Golang Web开发的开发者参考和实践。

## 功能模块

### 管理端

餐饮企业内部员工使用，主要功能有：

| 模块      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| 登录/退出 | 内部员工必须登录后，才可以访问系统管理后台                   |
| 员工管理  | 管理员可以在系统后台对员工信息进行管理，包含查询、新增、编辑、禁用等功能 |
| 分类管理  | 主要对当前餐厅经营的菜品分类或套餐分类进行管理维护，包含查询、新增、修改、删除等 |
| 菜品管理  | 主要维护各个分类下的菜品信息，包含查询、新增、修改、删除、启售、停售等功能 |
| 套餐管理  | 主要维护当前餐厅中的套餐信息，包含查询、新增、修改、删除、启售、停售等功能 |
| 订单管理  | 主要维护用户在移动端下的订单信息，包含查询、取消、派送、完成，以及订单报表下载等功能 |
| 数据统计  | 主要完成对餐厅的各类数据统计，如营业额、用户数量、订单等     |

### 用户端

| 模块        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| 登录/退出   | 对接微信小程序开放API接口实现微信授权登录                    |
| 点餐-菜单   | 可在点餐界面选择菜品分类或套餐分类，并根据当前选择的分类加载其中的菜品信息，供用户查询选择 |
| 点餐-购物车 | 用户选中的菜品就会加入用户的购物车，主要包含查询购物车、加入购物车、删除购物车、清空购物车等功能 |
| 订单支付    | 用户选完菜品/套餐后，可以对购物车菜品进行结算支付，这时就需要进行订单的支付 |
| 个人信息    | 用户个人信息界面提供历史订单和收货地址管理，可查看历史订单信息或使用再来一单功能，收货地址可设置默认地址、新增地址、修改地址、删除地址等功能 |

## 技术栈

- **后端框架**：Gin - 轻量级HTTP Web框架
- **ORM框架**：GORM - 对象关系映射框架
- **缓存**：Redis - 高性能的key-value存储系统
- **认证**：JWT - JSON Web Token认证机制
- **定时任务**：GoCron - Golang定时任务库
- **日志**：自定义日志系统
- **配置管理**：Viper - 配置解决方案
- **数据库**：MySQL - 关系型数据库
- **容器化**：Docker & Docker Compose - 应用容器引擎和多容器管理工具

## 快速开始

### 本地开发环境

1. 克隆项目到本地

```shell
git clone https://github.com/lairun857/cangqiong.git
cd cangqiong
```

2. 安装依赖

```shell
cd sky-take-out-go
go mod tidy
```

如果下载缓慢，可以配置国内镜像源：

```shell
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

3. 准备运行环境

- 安装并启动MySQL数据库
- 执行`script/sky.sql`脚本创建数据库和基础数据
- 安装并启动Redis服务

4. 修改配置文件

根据本地环境修改`config/application-dev.yaml`中的数据库和Redis配置

5. 启动服务

```shell
# 使用开发环境配置启动
go run main.go

# 或指定release配置文件启动
go run main.go --env=release
```

6. 启动前端服务

```shell
# 切换到web前端目录
cd client
# 启动nginx服务
.\nginx.exe
```

7. 访问系统

打开浏览器访问：http://localhost

### Docker部署

1. 克隆项目到本地

```shell
git clone https://github.com/lairun857/cangqiong.git
cd cangqiong/sky-take-out-go
```

2. 创建配置文件需要的共享卷

```shell
mkdir -p /home/running/takeout/config
mkdir -p /home/running/takeout/logs
```

3. 拷贝配置文件到共享卷

```shell
cp ./config/*.yaml /home/running/takeout/config/
```

4. 使用Docker Compose启动服务

```shell
docker-compose up -d
```

## 项目结构

```
sky-take-out-go/
├── client/                 # WEB客户端
├── common/                 # 通用包
│   ├── e/                  # 自定义错误、错误code、消息
│   ├── enum/               # 自定义枚举、常量、变量
│   ├── utils/              # 工具包(JWT、限流、邮件等)
│   └── result.go           # 通用数据返回格式
├── config/                 # 配置文件
├── global/                 # 全局变量
├── initialize/             # 初始化组件
├── internal/               # 内部包
│   ├── api/                # API层
│   │   ├── controller/     # 控制器
│   │   ├── request/        # 请求模型
│   │   └── response/       # 响应模型
│   ├── model/              # 数据模型
│   ├── repository/         # 数据访问层
│   ├── router/             # 路由定义
│   └── service/            # 业务逻辑层
├── logger/                 # 日志管理
├── middle/                 # 中间件
├── script/                 # 脚本文件
├── docker-compose.yaml     # Docker Compose配置
├── dockerfile              # Docker构建文件
├── go.mod                  # Go模块依赖
└── main.go                 # 程序入口
```

## 开发指南

### 项目架构

本项目采用经典的三层架构：

1. **表示层（Controller）**：处理HTTP请求和响应
2. **业务逻辑层（Service）**：实现业务逻辑
3. **数据访问层（Repository）**：负责数据库操作

### 开发规范

- **命名规范**：遵循Go语言命名规范，使用驼峰命名法
- **错误处理**：统一使用项目定义的错误处理机制
- **日志记录**：使用全局日志对象记录关键操作和错误信息
- **事务处理**：在Service层处理事务逻辑

## 贡献指南

1. Fork 本仓库
2. 创建您的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交您的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 打开一个 Pull Request

## 许可证

本项目采用 MIT 许可证，详情请参阅 [LICENSE](LICENSE) 文件。

## 联系方式

如有问题或建议，请通过以下方式联系：

- GitHub Issues: [https://github.com/lairun857/cangqiong/issues](https://github.com/lairun857/cangqiong/issues)
- 交流群：828448599
