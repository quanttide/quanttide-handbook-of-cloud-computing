# Serverless Framework部署函数计算

## `serverless.yml`配置

Serverless Framework参数列表：
- https://github.com/serverless-components/tencent-multi-scf/blob/master/docs/configure.md

## 权限设置

子账号授权参考：

- https://cloud.tencent.com/document/product/1154/43006#sls_qcsrole-.E8.A7.92.E8.89.B2.E6.9D.83.E9.99.90.E5.88.97.E8.A1.A8-.3Ca-id.3D.22list.22.3E.3C.2Fa.3E

特别注意第4、5步。

## 存储和数据库

- DB：基于实例和数据库隔离。目前的实践是不同项目用数据库隔离。开发和生产没有隔离，计算资源可能岔，不够安全。
- 私有网络：同一个项目在一个，目前的实践是跟着DB搞，都得设置一个私有网络，不太安全。
- COS：同地域，内网省流量。
- CSF：没啥太多要注意的，就有一些Python的写法。
