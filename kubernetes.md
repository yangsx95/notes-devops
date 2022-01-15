# Kubernetes

## Pod

### 控制器管理的Pod

#### ReplicaSet

#### Deployment

#### DaemonSet

## Service

## API访问控制

Kubernetes作为分布式集群管理工具，保证集群的安全性是一个重要的任务。API Server是集群内部各个组件的通讯中介，也是外部控制的入口。所以Kubernetes的安全机制基本就是围绕 API Server来设计的。Kubernetes使用 **认证**、**鉴权**、**准入控制**三个步骤来保证API Server的安全。

![Kubernetes API 请求处理步骤示意图](images/access-control-overview.svg)

### Authentication

认证，用于保证通讯双方是可信的

#### 集群用户

在k8s集群中有两类用户：

- 由k8s管理的服务账号
- 与k8s集群无关的服务，通常由应用程序自己单独管理，比如wordpress的用户

#### 身份认证策略

对集群用户的身份认证策略，k8s提供了如下几种方式：

- 客户端证书
- Bearer Token（持有者令牌）
  - Service Account Token
  - Bootstrap Token
  - Static Token
- Proxy（身份认证代理）
- HTTP基本认证

#### 证书认证

1. 生成csr(Certificate Signing Request 证书请求)文件，csr中包含了你生成的公钥，以及域名、公私、地址等信息

   ```shell
   # 在客户端本机生成key，也是私钥，其中2048是秘钥长度
   openssl genrsa -out myk8s.key 2048
   # 生成证书请求文件(csr)
   # req代表证书请求，
   # -new代表生成，
   # -key指定刚刚生成的私钥文件(会根据私钥生成公钥) 
   # -nodes 表示不对私钥加密
   # -out指定csr文件输出路径
   openssl req -new -key myk8s.key -nodes -out myk8s.csr
   # 控制台将会要求输入以下参数：国家、省、市、组织、域名、邮箱等
   ```

   



### Authorization

用户在认证后，然后去请求资源，需要先进行鉴权，确认请求方有哪些资源权限。API Server目前支持以下几种授权策略：

- AlwaysDeny：拒绝所有请求，一般用于测试
- AlwayAllow：允许所有请求，如果集群不需要授权流程，可以采用该种策略
- ABAC：Attribute-Based Access Control，基于属性的访问控制，表示使用用户配置的授权规则对用户请求进行匹配和控制
- Webbook：通过调用外部的Rest服务对用户进行更改
- RBAC：Role-Based Access Control，默认规则，基于角色的访问控制

#### RBAC

基于角色的访问控制，拥有以下优势：

- 集群资源覆盖全
- RBAC由资源对象完成，可以用kubectl或API进行操作
- 可以在运行时进行调整，无需重启API Server

**资源对象说明：**



