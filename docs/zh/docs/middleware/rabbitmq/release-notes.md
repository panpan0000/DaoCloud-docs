# RabbitMQ Release Notes

本页列出 RabbitMQ 消息队列的 Release Notes，便于您了解各版本的演进路径和特性变化。

## 2023-03-30

### v0.9.1

#### 优化

- **优化** `mcamel-rabbitmq` 改进 Operator 镜像为加速地址。

## 2023-03-28

### v0.9.0

#### 新功能

- **新增** `mcamel-rabbitmq` 支持中间件链路追踪适配。
- **新增** 在安装 `mcamel-rabbitmq` 根据参数配置启用链路追踪。

## 2023-02-23

### v0.8.0

#### 新功能

- **新增** `mcamel-rabbitmq` helm-docs 模板文件。
- **新增** `mcamel-rabbitmq` 应用商店中的 Operator 只能安装在 mcamel-system。
- **新增** `mcamel-rabbitmq` 支持 cloud shell。
- **新增** `mcamel-rabbitmq` 支持导航栏单独注册。

- **新增** 日志查看操作说明，支持自定义查询、导出等功能。

#### 升级

- **升级** `mcamel-rabbitmq` 升级离线镜像检测脚本。

#### 修复

- **新增** `mcamel-rabbitmq` 支持查看日志。
- **修复** `mcamel-rabbitmq` 实例名太长导致自定义资源无法创建的问题。
- **修复** `mcamel-rabbitmq` 工作空间 Editor 用户无法查看实例密码。

## 2022-12-25

### v0.7.0

#### 新功能

- **新增** `mcamel-rabbitmq` NodePort 端口冲突提前检测。
- **新增** `mcamel-rabbitmq` 节点亲和性配置。

#### 优化

- **优化** `mcamel-rabbitmq-ui` 中间件样式走查优化。  

## 2022-11-28

### v0.6.4

#### 新功能

- **新增** 获取用户列表接口
- **新增** 支持多架构的镜像，配置方式为 `depend.arm64-img.rabbitClusterImageFormat: xxxx`
- **新增** 支持 sc 扩容拦截，当 sc 不支持扩容的时候，直接拦截掉
- **新增** 返回列表或者详情时的公共字段
- **新增** 返回 alerts
- **新增** 校验 Service 注释

#### 优化

- **优化** 密码校验调整为 MCamel 中等密码强度

#### 修复

- **修复** 页面控制台可能访问到错误的端口

## 2022-10-27

### v0.6.1

#### 新功能

- **新增** 增加覆盖率
- **新增** 前端的 UI 注册功能
- **新增** 性能增强
- **新增** 列表页增加分页功能
- **新增** 增加修改配置的功能
- **新增** 增加返回可修改配置项的功能
- **新增** 更改创建实例的限制为集群级别，原来为 namespace 级别
- **新增** 增加监控地址的拼接功能
- **新增** 增加可以修改版本号的功能
- **新增** 修改底层 update 逻辑为 patch 逻辑
- **新增** RabbitMQ e2e 测试覆盖率 17.24% 左右
- **新增** 增加 RabbitMQ 性能压测报告
- **新增** 增加 RabbitMQ bug 抽查
- **新增** 对接 ghippo 增加 workspace 接口
- **新增** 对接 insight 通过 crd 注入 dashboard
- **新增** 将时间戳 api 字段统一调整为 int64
- **新增** 单测覆盖率提升到 53%
- **优化** 更新 release note 脚本，执行 release-process 规范
- **新增** 新增功能说明
- **新增** 创建 RabbitMQ
- **新增** RabbitMQ 数据迁移
- **新增** 实例监控
- **新增** 首次进入 RabbitMQ
- **新增** 适用场景
