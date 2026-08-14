# Serverless Framework部署函数计算

这一页记录如何用 Serverless Framework 部署函数计算项目。它更像一个部署约定清单，而不是完整教程。

## 适用范围

- 需要部署腾讯云函数或类似函数计算平台。
- 需要通过 `serverless.yml` 描述项目资源。
- 需要在 CI 中生成 `.env` 并执行部署。

## `serverless.yml`配置

Serverless Framework参数列表：
- https://github.com/serverless-components/tencent-multi-scf/blob/master/docs/configure.md

## 权限设置

子账号授权参考：

- https://cloud.tencent.com/document/product/1154/43006#.E5.AD.90.E8.B4.A6.E5.8F.B7.E6.9D.83.E9.99.90.E9.85.8D.E7.BD.AE.3Ca-id.3D.22rightsprofile.22.3E.3C.2Fa.3E

特别注意第4、5步。

## 存储和数据库

- DB：基于实例和数据库隔离。目前的实践是不同项目用数据库隔离。开发和生产没有隔离，计算资源可能岔，不够安全。
- 私有网络：同一个项目在一个，目前的实践是跟着DB搞，都得设置一个私有网络，不太安全。
- COS：同地域，内网省流量。
- CSF：没啥太多要注意的，就有一些Python的写法。

## 快速检查

- `serverless.yml` 是否能读取所需环境变量
- 子账号授权是否完成
- DB、私有网络、COS 和云函数是否按环境分开
- 本地和 CI 的配置是否一致
