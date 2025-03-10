# 集群配置文件 clusterConfig.yaml

此 YAML 文件包含了集群的各项配置字段，安装之前必须先配置此文件。
该文件将定义部署模式、集群节点信息等关键参数。默认位于 `offline/sample/` 目录。

v0.6.0 版本对配置文件的结构进行了优化，相对之前更加清晰，易读。

## ClusterConfig 示例

以下是一个 ClusterConfig 文件示例。

```yaml title="clusterConfig.yaml"
apiVersion: provision.daocloud.io/v1alpha3
kind: ClusterConfig
metadata:
spec:
  clusterName: my-cluster
   
  #火种节点的域名或IP，默认解析为火种节点默认网关所在网卡的IP；可手动填入IP或域名，若为域名，如果检测到无法解析，将自动建立此域名和火种节点默认IP的映射
  #bootstrapNode: auto
 
  loadBalancer:
 
    # NodePort(default), metallb, cloudLB (Cloud Controller)
    type: metallb
    istioGatewayVip: xx.xx.xx.xx/32 # 当 loadBalancer.type 是 metallb 时必填，为 DCE 提供 UI 和 OpenAPI 访问权限
    insightVip: xx.xx.xx.xx/32 # 别丢弃/32, 当 loadBalancer.type 是 metallb 时必填，用作 GLobal 集群的 Insight 数据采集入口，子集群的 insight-agent 可以向这个 VIP 报告数据
 
  # # 指定 ssh 私钥，定义后无需再定义节点的 ansibleUser、ansiblePass
  # privateKeyPath: /root/.ssh/id_rsa_sample
 
  masterNodes:
    - nodeName: "g-master1" # Node Name will override the hostName, should align with RFC1123 stsandard
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
    - nodeName: "g-master2"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
    - nodeName: "g-master3"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
  workerNodes:
    - nodeName: "g-worker1"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
      nodeTaints:                       # for 7 node mode: at least 3 worker nodes should carry below taint(ES-Only nodes)
       - "node.daocloud.io/es-only=true:NoSchedule"
    - nodeName: "g-worker2"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
      nodeTaints:
       - "node.daocloud.io/es-only=true:NoSchedule"
    - nodeName: "g-worker3"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
      nodeTaints:
       - "node.daocloud.io/es-only=true:NoSchedule"
 
  #ntpServer:
  #  - 0.pool.ntp.org
  #  - ntp1.aliyun.com
  #  - ntp.ntsc.ac.cn
  
  fullPackagePath: "/root/offline" # 解压后的离线包的路径，离线模式下该字段必填
  
  osRepos: # 操作系统软件源
 
    # 支持 official-service(default), builtin
    type: builtin
    isoPath: "/root/CentOS-7-x86_64-DVD-2009.iso"
    osPackagePath: "/root/os-pkgs-centos7-v0.4.4.tar.gz"
 
    #type: external
    # Optional only if external repo already have full required resources
    #isoPath: "/root/CentOS-7-x86_64-DVD-2009.iso"
    # Optional only if external repo already have full required resources
    #osPackagePath: "/root/os-pkgs-centos7-v0.4.4.tar.gz"
    # `centos` as CentOS, RedHat,kylin AlmaLinux or Fedora
    # `debian` as Debian
    # `ubuntu` as Ubuntu
    #externalRepoType: centos
    #externalRepoURLs: ['https://extertal-repo.daocloud.io/centos/\$releasever/os/\$basearch/']
 
  imagesAndCharts: # 镜像仓库和 Chart仓库源
 
    # official-service(default), builtin or external
 
    type: builtin
 
    #type: external
    # IP or domain name
    #externalImageRepo: https://external-registry.daocloud.io
    # Set user and password. Optional
    #externalImageRepoUsername: admin
    #externalImageRepoPassword: Harbor12345
    # chartmuseum or harbor
    #externalChartRepoType: chartmuseum
    # IP or domain name
    #externalChartRepo: https://external-charts.daocloud.io:8081
    # Set user and password. Optional
    #externalChartRepoUsername: rootuser
    #externalChartRepoPassword: rootpass123
 
  addonPackage: # 应用商店 addon 包，定义后会对 addon 进行离线化
    #path: "/root/addon-offline-full-package-v0.4.8-amd64.tar.gz"
   
binaries: # 二进制可执行文件
 
    # official-service(default), builtin
    type: builtin
 
    #type: external
    # IP or domain name
    #externalRepository: https://external-binaries.daocloud.io:9000/kubean
 
  # Examples as below. More refer to kubespray options setting documentations.
  #kubeanConfig: |-
  #  bin_dir: /usr/local/bin
  #  http_proxy: ""
  #  https_proxy: ""
  #  upstream_dns_servers:
  #    - 8.8.8.8
  #    - 8.8.4.4
 
  # k8sVersion only take effect in online mode, don't set it in offline mode.
  # Unless to install a non-latest k8s version with offline pkg in place.
  #k8sVersion: v1.25.4
  #auditConfig:
  #  logPath: /var/log/audit/kube-apiserver-audit.log
  #  logHostPath: /var/log/kubernetes/audit
  #  #policyFile: /etc/kubernetes/audit-policy/apiserver-audit-policy.yaml
  #  #logMaxAge: 30
  #  #logMaxBackups: 10
  #  #logMaxSize: 100
  #  #policyCustomRules: >
  #  #  - level: None
  #  #    users: []
  #  #    verbs: []
  #  #    resources: []
  #network:
  #  cni: calico
  #  clusterCIDR: 10.233.64.0/18
  #  serviceCIDR: 10.233.0.0/18
  #cri:
  #  criProvider: containerd
  #  # criVersion only take effect in online mode, don't set it in offline mode
  #  #criVersion: 1.6.8
```

## 关键字段

该 YAML 文件中的关键字段说明，请参阅下表。

| 字段                                      | 说明                                                         | 默认值                                                  |
| :---------------------------------------- | :----------------------------------------------------------- | :------------------------------------------------------ |
| auditConfig                               | k8s api-server 的审计日志配置                                | 默认关闭                                                |
| binaries                                  | 二进制可执行文件                                             | -                                                       |
| binaries.externalRepository               | 外置二进制可执行文件仓库的访问地址，URL形式                  | -                                                       |
| binaries.type                             | 二进制可执行文件的访问模式，取值为 official-service(在线), builtin(火种节点内置的minio) | official-service                                        |
| clusterName                               | 在 KuBean Cluster 里的 Global 集群命名                       | -                                                       |
| fullPackagePath                           | 解压后的离线包的路径，离线模式下该字段必填                   | -                                                       |
| imagesAndCharts                           | 镜像仓库和 Chart仓库源                                       | -                                                       |
| imagesAndCharts.externalChartRepo         | 外置Chart仓库的IP或域名                                      | -                                                       |
| imagesAndCharts.externalChartRepoPassword | 外置Chart仓库的密码，用于推送镜像                            | -                                                       |
| imagesAndCharts.externalChartRepoType     | 外置Chart仓库的类型，取值为 chartmuseum，harbor              | -                                                       |
| imagesAndCharts.externalChartRepoUsername | 外置Chart仓库的用户名，用于推送镜像                          | -                                                       |
| imagesAndCharts.externalImageRepo         | 指定external仓库的IP或者域名(需指定协议头)                   | -                                                       |
| imagesAndCharts.externalImageRepoPassword | 外置镜像仓库的密码，用于推送镜像                             | -                                                       |
| imagesAndCharts.externalImageRepoUsername | 外置镜像仓库的用户名，用于推送镜像                           | -                                                       |
| imagesAndCharts.type                      | 镜像与Chart的访问模式，取值为 official-service(在线), buitin(火种内置 registry 和 chartmuseum), external(外置) | official-service                                        |
| k8sVersion                                | kuBean 安装集群的 K8s 版本必须跟 KuBean 和离线包相匹配       | -                                                       |
| loadBalancer.insightVip                   | 如果负载均衡模式是 metallb，则需要指定一个 VIP，供给 GLobal 集群的 insight 数据收集入口使用，子集群的 insight-agent 可上报数据到这个 VIP | -                                                       |
| loadBalancer.istioGatewayVip              | 如果负载均衡模式是 metallb，则需要指定一个 VIP，供给 DCE 的 UI 界面和 OpenAPI 访问入口 | -                                                       |
| loadBalancer.type                         | 所使用的 LoadBalancer 的模式，物理环境用 metallb，POC 用 NodePort，公有云和 SDN CNI 环境用 cloudLB | NodePort (default)、metallb、cloudLB (Cloud Controller) |
| masterNodes                               | Global 集群：Master 节点列表，包括 nodeName/ip/ansibleUser/ansiblePass 几个关键字段 | -                                                       |
| network.clusterCIDR                       | Cluster CIDR                                                 | -                                                       |
| network.cni                               | CNI 选择，比如 Calico、Cilium                                | calico                                                  |
| network.serviceCIDR                       | Service CIDR                                                 | -                                                       |
| ntpServer                                 | 可用的 NTP 服务器，供给新节点同步时间                        | -                                                       |
| osRepos                                   | 操作系统软件源                                               | -                                                       |
| osRepos.externalRepoType                  | 外置软件源服务的操作系统类型, 取值为 centos(所有红帽系列), debian, ubuntu | -                                                       |
| osRepos.externalRepoURLs                  | 外置软件源的访问地址                                         | -                                                       |
| osRepos.isoPath                           | 操作系统 ISO 文件的路径, type 为 builtin 时不能为空          | -                                                       |
| osRepos.osPackagePath                     | 系统包文件的路径 ，type 为 builtin 时不能为空                | -                                                       |
| osRepos.type                              | 操作系统软件源的访问模式，取值为 official-service(在线), builtin(火种节点内置的minio) | official-service                                        |
| privateKeyPath                            | kuBean 部署集群的 SSH 私钥文件路径，如果填写则不需要定义ansibleUser、ansiblePass | -                                                       |
| workerNodes                               | Global 集群：Worker 节点列表，包括 nodeName/ip/ansibleUser/ansiblePass 几个关键字段 | -                                                       |

## 精简配置说明

**离线模式下采用 builtin 方式安装**

```yaml
apiVersion: provision.daocloud.io/v1alpha3
kind: ClusterConfig
metadata:
  creationTimestamp: null
spec:
  clusterName: my-cluster
  masterNodes:
    # Node Name will override the hostName, should align with RFC1123 standard
    - nodeName: "g-master1"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
  workerNodes:
  fullPackagePath: "/root/offline"
  osRepos:
    # official-service(if omit or empty), builtin or external
    type: builtin
    isoPath: "/root/CentOS-7-x86_64-DVD-2009.iso"
    osPackagePath: "/root/os-pkgs-centos7-v0.4.4.tar.gz"
  imagesAndCharts:
    # official-service(if omit or empty), builtin or external
    # Not Support External S3 so far...... FIXME
    type: builtin
  addonPackage:
    path: "/root/addon-offline-full-package-v0.4.8-amd64.tar.gz"
  binaries:
    # official-service(if omit or empty), builtin or external
    type: builtin
```

**离线模式下采用 external 方式安装**

```yaml
apiVersion: provision.daocloud.io/v1alpha3
kind: ClusterConfig
metadata:
  creationTimestamp: null
spec:
  clusterName: my-cluster
  masterNodes:
    # Node Name will override the hostName, should align with RFC1123 standard
    - nodeName: "g-master1"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
  workerNodes:
    
  fullPackagePath: "/root/offline"
  osRepos:
    # official-service(if omit or empty), builtin or external
    type: external
    # Optional only if external repo already have full required resources
    isoPath: "/root/CentOS-7-x86_64-DVD-2009.iso"
    # Optional only if external repo already have full required resources
    osPackagePath: "/root/os-pkgs-centos7-v0.4.4.tar.gz"
    # `centos` as CentOS, RedHat,kylin AlmaLinux or Fedora
    # `debian` as Debian
    # `ubuntu` as Ubuntu
    externalRepoType: centos
    externalRepoURLs: ["https://extertal-repo.daocloud.io/centos/\$releasever/os/\$basearch/"]
  imagesAndCharts:
    # official-service(if omit or empty), builtin or external
    # Not Support External S3 so far...... FIXME
    type: external
    # Optional only if external repo already have full required resources
    # IP or domain name
    externalImageRepo: https://external-registry.daocloud.io
    externalImageRepoUsername: admin
    externalImageRepoPassword: Harbor12345
    # chartmuseum or harbor
    externalChartRepoType: chartmuseum
    # IP or domain name
    externalChartRepo: https://external-charts.daocloud.io:8081
    externalChartUsername: rootuser
    externalChartMuseumPassword: rootpass123
  addonPackage:
    path: "/root/addon-offline-full-package-v0.4.8-amd64.tar.gz"
  binaries:
    # official-service(if omit or empty), builtin or external
    type: external
    # Optional only if external repo already have full required resources
    # IP or domain name
    externalRepository: https://external-binaries.daocloud.io:9000/kubean
```

**在线模式采用 official-service 方式安装**

```yaml
apiVersion: provision.daocloud.io/v1alpha3
kind: ClusterConfig
metadata:
  creationTimestamp: null
spec:
  clusterName: my-cluster
  masterNodes:
    # Node Name will override the hostName, should align with RFC1123 standard
    - nodeName: "g-master1"
      ip: xx.xx.xx.xx
      ansibleUser: "root"
      ansiblePass: "dangerous"
  workerNodes:
```

## 通过命令行生成 clusterConfig 配置文件模板

### 全模式 1节点模式

``` bash
# 官方在线:
./dce5-installer generate-config --install-mode=cluster-create --master=1 --access-type=official-service
# 官方在线简化版：
./dce5-installer generate-config --master=1

# 内建离线:
./dce5-installer generate-config --install-mode=cluster-create --master=1 --access-type=builtin
# 内建离线简化版：
./dce5-installer generate-config --master=1 --access-type=builtin

# 扩展离线:
./dce5-installer generate-config --install-mode=cluster-create --master=1 --access-type=external
# 扩展离线简化版：
./dce5-installer generate-config --master=1 --access-type=external

```
 
### 全模式 4节点模式

``` bash
# 官方在线:
./dce5-installer generate-config --install-mode=cluster-create --master=3 --access-type=official-service
# 官方在线简化版：
./dce5-installer generate-config --master=3

# 内建离线:
./dce5-installer generate-config --install-mode=cluster-create --master=3 --access-type=builtin
# 内建离线简化版：
./dce5-installer generate-config --master=3 --access-type=builtin

# 扩展离线:
./dce5-installer generate-config --install-mode=cluster-create --master=3 --access-type=external
# 扩展离线简化版：
./dce5-installer generate-config --master=3 --access-type=external
```

### 全模式 7节点模式

``` bash
# 官方在线:
./dce5-installer generate-config --install-mode=cluster-create --master=3 --worker=3 --access-type=official-service
# 官方在线简化版：
./dce5-installer generate-config --master=3 --worker=3

# 内建离线:
./dce5-installer generate-config --install-mode=cluster-create --master=3 --worker=3 --access-type=builtin
# 内建离线简化版：
./dce5-installer generate-config --master=3 --worker=3 --access-type=builtin

# 扩展离线:
./dce5-installer generate-config --install-mode=cluster-create --master=3 --worker=3 --access-type=external
# 扩展离线简化版：
./dce5-installer generate-config --master=3 --worker=3 --access-type=external
```
 
### 社区版

``` bash
# 官方在线
./dce5-installer generate-config --install-mode=install-app --access-type=official-service
# 官方在线简化版：
./dce5-installer generate-config --install-mode=install-app

# 内建离线:
./dce5-installer generate-config --install-mode=install-app --access-type=builtin

# 扩展离线:
./dce5-installer generate-config --install-mode=install-app --access-type=external
```
