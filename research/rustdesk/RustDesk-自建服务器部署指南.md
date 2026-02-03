# RustDesk 自建服务器部署指南

> 本文档详细介绍如何自建 RustDesk 远程桌面服务器，实现安全、高效、无限制的远程控制体验。

## 目录

- [1. 概述](#1-概述)
  - [1.1 什么是 RustDesk](#11-什么是-rustdesk)
  - [1.2 为什么要自建服务器](#12-为什么要自建服务器)
  - [1.3 架构说明](#13-架构说明)
- [2. 准备工作](#2-准备工作)
  - [2.1 服务器要求](#21-服务器要求)
  - [2.2 端口规划](#22-端口规划)
  - [2.3 防火墙配置](#23-防火墙配置)
- [3. 服务端部署](#3-服务端部署)
  - [3.1 方案一：Docker 部署（推荐）](#31-方案一docker-部署推荐)
  - [3.2 方案二：二进制直接部署](#32-方案二二进制直接部署)
- [4. 客户端配置](#4-客户端配置)
  - [4.1 下载客户端](#41-下载客户端)
  - [4.2 配置连接](#42-配置连接)
  - [4.3 验证连接](#43-验证连接)
- [5. 进阶配置](#5-进阶配置)
  - [5.1 使用域名](#51-使用域名)
  - [5.2 固定密钥](#52-固定密钥)
  - [5.3 设置访问密码](#53-设置访问密码)
  - [5.4 HTTPS/WSS 支持](#54-httpswss-支持)
- [6. 运维管理](#6-运维管理)
  - [6.1 日志查看](#61-日志查看)
  - [6.2 服务监控](#62-服务监控)
  - [6.3 备份与恢复](#63-备份与恢复)
- [7. 常见问题](#7-常见问题)
- [8. 参考资料](#8-参考资料)

---

## 1. 概述

### 1.1 什么是 RustDesk

RustDesk 是一款开源的远程桌面软件，使用 Rust 语言开发，具有以下特点：

- **开源免费**：代码完全开源，无任何功能限制
- **跨平台支持**：支持 Windows、macOS、Linux、iOS、Android
- **高性能**：Rust 语言保证了内存安全和高性能
- **端到端加密**：保护数据传输安全
- **支持自建服务器**：完全掌控自己的数据

GitHub 仓库：
- 客户端：https://github.com/rustdesk/rustdesk
- 服务端：https://github.com/rustdesk/rustdesk-server

### 1.2 为什么要自建服务器

| 原因 | 说明 |
|------|------|
| **官方暂停国内服务** | 因被诈骗分子滥用，官方已暂停中国区公共服务器 |
| **降低延迟** | 国内服务器延迟可从 200ms+ 降至 20ms 以内 |
| **数据安全** | 数据不经过第三方服务器，完全自主可控 |
| **无限制使用** | 不限画质、帧率、连接数、使用时长 |
| **成本更低** | 相比商业软件会员费，自建成本更低 |

### 1.3 架构说明

RustDesk 服务端由两个核心组件组成：

```
┌─────────────────────────────────────────────────────────────┐
│                        网络拓扑                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│    ┌──────────┐                         ┌──────────┐       │
│    │ 客户端 A │◄─────── P2P 直连 ───────►│ 客户端 B │       │
│    │ (控制端) │                         │ (被控端) │       │
│    └────┬─────┘                         └────┬─────┘       │
│         │                                    │             │
│         │      NAT 打洞失败时走中继           │             │
│         │                                    │             │
│         ▼                                    ▼             │
│    ┌─────────────────────────────────────────────┐         │
│    │              自建服务器                      │         │
│    │  ┌─────────────────┐  ┌─────────────────┐  │         │
│    │  │      hbbs       │  │      hbbr       │  │         │
│    │  │  ID/信令服务器   │  │    中继服务器    │  │         │
│    │  │                 │  │                 │  │         │
│    │  │ - ID 注册       │  │ - 数据中继      │  │         │
│    │  │ - NAT 打洞      │  │ - P2P 失败时    │  │         │
│    │  │ - 心跳维护      │  │   转发流量      │  │         │
│    │  └─────────────────┘  └─────────────────┘  │         │
│    └─────────────────────────────────────────────┘         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**工作流程：**

1. 客户端启动后向 hbbs 注册，获取唯一 ID
2. 控制端请求连接被控端时，hbbs 协助进行 NAT 打洞
3. 打洞成功则 P2P 直连（延迟最低）
4. 打洞失败则通过 hbbr 中继转发（依赖服务器带宽）

---

## 2. 准备工作

### 2.1 服务器要求

| 项目 | 最低要求 | 推荐配置 |
|------|----------|----------|
| CPU | 1 核 | 2 核+ |
| 内存 | 512 MB | 1 GB+ |
| 带宽 | 1 Mbps | 3 Mbps+（中继时更流畅） |
| 系统 | Linux (推荐) | Ubuntu 20.04+ / CentOS 7+ |
| 网络 | 公网 IP | 固定公网 IP |

**推荐云服务商：**

| 服务商 | 产品 | 参考价格 |
|--------|------|----------|
| 阿里云 | 轻量应用服务器 | 99 元/年起 |
| 腾讯云 | 轻量应用服务器 | 99 元/年起 |
| 华为云 | 弹性云服务器 | 按量付费 |
| 雨云 | 云服务器 | 价格较低 |

> **建议**：选择国内服务器可获得更低延迟，选择地理位置靠近使用地点的机房。

### 2.2 端口规划

RustDesk 服务端需要以下端口：

| 端口 | 协议 | 组件 | 用途 |
|------|------|------|------|
| 21115 | TCP | hbbs | NAT 类型测试 |
| 21116 | TCP | hbbs | NAT 打洞与连接服务 |
| 21116 | UDP | hbbs | ID 注册与心跳服务 |
| 21117 | TCP | hbbr | 中继服务 |
| 21118 | TCP | hbbs | Web 客户端支持 |
| 21119 | TCP | hbbr | Web 客户端支持 |

> **注意**：21116 端口需要同时开放 TCP 和 UDP 协议。

### 2.3 防火墙配置

#### 云服务器安全组配置

在云服务器控制台的安全组中添加以下规则：

| 方向 | 协议 | 端口范围 | 来源 |
|------|------|----------|------|
| 入站 | TCP | 21115-21119 | 0.0.0.0/0 |
| 入站 | UDP | 21116 | 0.0.0.0/0 |

#### Linux 系统防火墙

**firewalld (CentOS/RHEL)：**

```bash
# 开放端口
firewall-cmd --permanent --add-port=21115-21119/tcp
firewall-cmd --permanent --add-port=21116/udp

# 重载配置
firewall-cmd --reload

# 验证
firewall-cmd --list-ports
```

**ufw (Ubuntu/Debian)：**

```bash
# 开放端口
ufw allow 21115:21119/tcp
ufw allow 21116/udp

# 启用防火墙
ufw enable

# 查看状态
ufw status
```

**iptables：**

```bash
# 开放端口
iptables -A INPUT -p tcp --dport 21115:21119 -j ACCEPT
iptables -A INPUT -p udp --dport 21116 -j ACCEPT

# 保存规则
# CentOS
service iptables save
# Ubuntu
iptables-save > /etc/iptables.rules
```

---

## 3. 服务端部署

### 3.1 方案一：Docker 部署（推荐）

Docker 部署是最简单、最推荐的方式，约 5 分钟即可完成。

#### 3.1.1 安装 Docker

```bash
# 一键安装 Docker
curl -fsSL https://get.docker.com | sh

# 启动 Docker 服务
systemctl start docker
systemctl enable docker

# 验证安装
docker --version
```

#### 3.1.2 安装 Docker Compose

```bash
# 下载 docker-compose
curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 添加执行权限
chmod +x /usr/local/bin/docker-compose

# 验证安装
docker-compose --version
```

#### 3.1.3 创建部署目录和配置文件

```bash
# 创建工作目录
mkdir -p /opt/rustdesk
cd /opt/rustdesk

# 创建数据目录
mkdir -p data
```

创建 `docker-compose.yml` 文件：

```yaml
version: '3'                                                                                                                                            
                                                                                                                                                        
services:                                                                                                                                               
  hbbs:                                                                                                                                                 
    container_name: hbbs                                                                                                                                
    image: rustdesk/rustdesk-server:latest                                                                                                              
    command: hbbs -r 16.162.60.39:21117                                                                                                                 
    network_mode: "host"                                                                                                                                
    volumes:                                                                                                                                            
      - ./data:/root                                                                                                                                    
    restart: unless-stopped                                                                                                                             
                                                                                                                                                        
  hbbr:                                                                                                                                                 
    container_name: hbbr                                                                                                                                
    image: rustdesk/rustdesk-server:latest                                                                                                              
    command: hbbr                                                                                                                                       
    network_mode: "host"                                                                                                                                
    volumes:                                                                                                                                            
      - ./data:/root                                                                                                                                    
    restart: unless-stopped 
```

> **重要**：将 `16.162.60.39` 替换为你的服务器公网 IP 地址。

#### 3.1.4 启动服务

```bash
# 拉取镜像并启动
docker-compose up -d

# 查看运行状态
docker-compose ps

# 查看日志
docker-compose logs -f
```

预期输出：

```
NAME      COMMAND                  STATUS          PORTS
hbbr      "hbbr"                   Up 10 seconds   0.0.0.0:21117->21117/tcp, 0.0.0.0:21119->21119/tcp
hbbs      "hbbs -r x.x.x.x:211…"   Up 10 seconds   0.0.0.0:21115-21116->21115-21116/tcp, 0.0.0.0:21116->21116/udp, 0.0.0.0:21118->21118/tcp




NAME      IMAGE                             COMMAND                  SERVICE   CREATED          STATUS          PORTS
hbbr      rustdesk/rustdesk-server:latest   "hbbr"                   hbbr      18 seconds ago   Up 18 seconds   0.0.0.0:21117->21117/tcp, [::]:21117->21117/tcp, 0.0.0.0:21119->21119/tcp, [::]:21119->21119/tcp
hbbs      rustdesk/rustdesk-server:latest   "hbbs -r 16.162.60.3…"   hbbs      18 seconds ago   Up 17 seconds   0.0.0.0:21115-21116->21115-21116/tcp, [::]:21115-21116->21115-21116/tcp, 0.0.0.0:21118->21118/tcp, [::]:21118->21118/tcp, 0.0.0.0:21116->21116/udp, [::]:21116->21116/udp
```

#### 3.1.5 获取公钥 (Key)

服务启动后会自动生成密钥对，获取公钥：

```bash
cat /opt/rustdesk/data/id_ed25519.pub
```

输出示例：

```
jdHLITyp6RZsZO0HD78aOjJDb5nh0IaXFMUx0MYdPrM=
```

> **重要**：保存这个公钥，客户端配置时需要使用。

#### 3.1.6 常用管理命令

```bash
# 停止服务
docker-compose down

# 重启服务
docker-compose restart

# 更新镜像
docker-compose pull
docker-compose up -d

# 查看实时日志
docker-compose logs -f hbbs
docker-compose logs -f hbbr
```

---

### 3.2 方案二：二进制直接部署

如果不想使用 Docker，可以直接部署二进制文件。

#### 3.2.1 下载服务端程序

```bash
# 创建目录
mkdir -p /opt/rustdesk
cd /opt/rustdesk

# 下载最新版本（请根据实际情况更新版本号）
# 查看最新版本：https://github.com/rustdesk/rustdesk-server/releases
VERSION="1.1.14"

# Linux AMD64
wget https://github.com/rustdesk/rustdesk-server/releases/download/${VERSION}/rustdesk-server-linux-amd64.zip

# 解压
unzip rustdesk-server-linux-amd64.zip

# 添加执行权限
chmod +x hbbs hbbr
```

> **国内下载加速**：如果 GitHub 下载慢，可使用代理：
> ```bash
> wget https://ghproxy.com/https://github.com/rustdesk/rustdesk-server/releases/download/${VERSION}/rustdesk-server-linux-amd64.zip
> ```

#### 3.2.2 手动启动（测试用）

```bash
# 启动 hbbr（中继服务器）
./hbbr &

# 启动 hbbs（ID/信令服务器）
./hbbs -r 16.162.60.39:21117 &
```

#### 3.2.3 配置 Systemd 服务（推荐）

创建 hbbs 服务文件 `/etc/systemd/system/hbbs.service`：

```ini
[Unit]
Description=RustDesk ID/Rendezvous Server
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/rustdesk
ExecStart=/opt/rustdesk/hbbs -r 16.162.60.39:21117
Restart=always
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

创建 hbbr 服务文件 `/etc/systemd/system/hbbr.service`：

```ini
[Unit]
Description=RustDesk Relay Server
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/rustdesk
ExecStart=/opt/rustdesk/hbbr
Restart=always
RestartSec=5
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

启用并启动服务：

```bash
# 重载 systemd 配置
systemctl daemon-reload

# 启用开机自启
systemctl enable hbbs hbbr

# 启动服务
systemctl start hbbs hbbr

# 查看状态
systemctl status hbbs hbbr
```

#### 3.2.4 获取公钥

```bash
cat /opt/rustdesk/id_ed25519.pub
```

---

## 4. 客户端配置

### 4.1 下载客户端

**官方下载地址：** https://github.com/rustdesk/rustdesk/releases

| 平台 | 下载文件 |
|------|----------|
| Windows | `rustdesk-x.x.x-x86_64.exe` |
| macOS | `rustdesk-x.x.x-x86_64.dmg` 或 `rustdesk-x.x.x-aarch64.dmg` (M系列芯片) |
| Linux | `rustdesk-x.x.x-x86_64.deb` / `.rpm` / `.AppImage` |
| Android | `rustdesk-x.x.x-arm64-v8a.apk` |
| iOS | App Store 搜索 "RustDesk" |

### 4.2 配置连接

#### Windows / macOS / Linux

1. 打开 RustDesk 客户端
2. 点击左上角 **菜单（三条横线）** → **设置**
3. 选择 **网络** 选项卡
4. 在 **ID 服务器** 栏填入：`你的服务器IP`
5. 在 **中继服务器** 栏填入：`你的服务器IP`（可留空，会使用 ID 服务器地址）
6. 在 **Key** 栏填入：`服务器生成的公钥内容`
7. 点击 **应用** 保存设置

#### Android / iOS

1. 打开 RustDesk App
2. 点击右下角 **设置** 图标
3. 选择 **ID/中继服务器**
4. 填入服务器地址和 Key
5. 保存设置

### 4.3 验证连接

配置完成后，检查以下状态：

1. **连接状态**：主界面右下角应显示 **"就绪"** 状态
2. **ID 显示**：应显示分配的数字 ID
3. **测试连接**：尝试在两台设备间进行远程连接

**连接状态说明：**

| 状态 | 说明 |
|------|------|
| 就绪 | 已成功连接到服务器 |
| 连接中... | 正在尝试连接服务器 |
| 连接超时 | 无法连接到服务器，检查网络和端口配置 |

---

## 5. 进阶配置

### 5.1 使用域名

使用域名代替 IP 地址，更加专业和方便管理。

#### 5.1.1 配置 DNS 解析

在域名管理后台添加 A 记录：

| 主机记录 | 记录类型 | 记录值 |
|----------|----------|--------|
| rustdesk | A | 你的服务器IP |

#### 5.1.2 修改服务配置

Docker 方式：修改 `docker-compose.yml`

```yaml
command: hbbs -r rustdesk.yourdomain.com:21117
```

二进制方式：修改 systemd 服务文件

```ini
ExecStart=/opt/rustdesk/hbbs -r rustdesk.yourdomain.com:21117
```

重启服务后，客户端使用域名配置即可。

### 5.2 固定密钥

默认情况下，密钥在首次启动时自动生成。如需使用固定密钥（便于迁移或多服务器同步），可手动生成：

```bash
cd /opt/rustdesk/data  # Docker 方式
# 或
cd /opt/rustdesk       # 二进制方式

# 生成 Ed25519 密钥对
openssl genpkey -algorithm ed25519 -out id_ed25519
openssl pkey -in id_ed25519 -pubout -out id_ed25519.pub

# 查看公钥
cat id_ed25519.pub
```

重启服务使新密钥生效。

### 5.3 设置访问密码

可以为服务器设置访问密码，增强安全性：

```bash
# hbbs 启动参数添加 -k
hbbs -r 16.162.60.39:21117 -k your_password
```

Docker 方式修改 `docker-compose.yml`：

```yaml
command: hbbs -r 16.162.60.39:21117 -k your_password
```

> **注意**：设置密码后，客户端配置时 Key 栏需要填入：`公钥@密码` 格式，或在单独的密码栏填入密码。

### 5.4 HTTPS/WSS 支持

如需为 Web 客户端启用 HTTPS，可配置 Nginx 反向代理：

```nginx
server {
    listen 443 ssl;
    server_name rustdesk.yourdomain.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://127.0.0.1:21118;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## 6. 运维管理

### 6.1 日志查看

#### Docker 方式

```bash
# 查看所有日志
docker-compose logs

# 实时查看日志
docker-compose logs -f

# 查看指定服务日志
docker-compose logs -f hbbs
docker-compose logs -f hbbr

# 查看最近 100 行
docker-compose logs --tail=100
```

#### 二进制方式

```bash
# 使用 journalctl 查看
journalctl -u hbbs -f
journalctl -u hbbr -f

# 查看最近日志
journalctl -u hbbs --since "1 hour ago"
```

### 6.2 服务监控

创建简单的健康检查脚本 `/opt/rustdesk/health_check.sh`：

```bash
#!/bin/bash

# 检查端口是否监听
check_port() {
    nc -z localhost $1
    return $?
}

# 检查各端口
ports=(21115 21116 21117 21118 21119)
all_ok=true

for port in "${ports[@]}"; do
    if check_port $port; then
        echo "[OK] Port $port is listening"
    else
        echo "[FAIL] Port $port is not listening"
        all_ok=false
    fi
done

if $all_ok; then
    echo "All services are running normally"
    exit 0
else
    echo "Some services have problems"
    exit 1
fi
```

配置定时检查（crontab）：

```bash
# 每 5 分钟检查一次
*/5 * * * * /opt/rustdesk/health_check.sh >> /var/log/rustdesk_health.log 2>&1
```

### 6.3 备份与恢复

#### 备份

需要备份的文件：

| 文件 | 说明 |
|------|------|
| `id_ed25519` | 私钥文件 |
| `id_ed25519.pub` | 公钥文件 |
| `docker-compose.yml` | Docker 配置（如使用） |

备份脚本：

```bash
#!/bin/bash
BACKUP_DIR="/backup/rustdesk"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p $BACKUP_DIR
tar -czvf $BACKUP_DIR/rustdesk_backup_$DATE.tar.gz \
    /opt/rustdesk/data/id_ed25519* \
    /opt/rustdesk/docker-compose.yml

# 保留最近 7 天的备份
find $BACKUP_DIR -name "*.tar.gz" -mtime +7 -delete
```

#### 恢复

```bash
# 解压备份
tar -xzvf rustdesk_backup_xxx.tar.gz -C /

# 重启服务
cd /opt/rustdesk
docker-compose restart
```

---

## 7. 常见问题

### Q1: 客户端显示"连接超时"或无法连接

**排查步骤：**

1. 检查服务是否运行：
   ```bash
   docker-compose ps  # Docker 方式
   systemctl status hbbs hbbr  # 二进制方式
   ```

2. 检查端口是否监听：
   ```bash
   netstat -tlnup | grep -E "2111[5-9]"
   ```

3. 检查防火墙和安全组：
   - 云服务器安全组是否开放端口
   - 系统防火墙是否开放端口
   - **特别注意 UDP 21116 端口**

4. 从外部测试端口：
   ```bash
   # 在其他机器上测试
   nc -zv your_server_ip 21116
   ```

### Q2: 能连接但非常卡顿

**可能原因及解决方案：**

1. **带宽不足**：升级服务器带宽（建议 3Mbps 以上）
2. **走中继而非 P2P**：
   - 检查两端网络 NAT 类型
   - 状态栏会显示连接类型（P2P 或 Relay）
3. **服务器距离远**：选择地理位置更近的服务器

### Q3: Key 配置错误

**检查项：**

1. 确保复制的是 `id_ed25519.pub` 文件的完整内容
2. 不要有多余的空格、换行符
3. 如果设置了密码，Key 格式为：`公钥内容` + 密码栏单独填写

### Q4: Docker 镜像拉取失败

**解决方案：**

1. 配置 Docker 镜像加速：
   ```bash
   # 编辑 /etc/docker/daemon.json
   {
     "registry-mirrors": [
       "https://docker.mirrors.ustc.edu.cn",
       "https://hub-mirror.c.163.com"
     ]
   }

   # 重启 Docker
   systemctl restart docker
   ```

2. 手动下载并导入镜像：
   ```bash
   # 在能访问的机器上
   docker pull rustdesk/rustdesk-server:latest
   docker save rustdesk/rustdesk-server:latest > rustdesk-server.tar

   # 传输到目标服务器后
   docker load < rustdesk-server.tar
   ```

### Q5: 如何升级服务端

**Docker 方式：**

```bash
cd /opt/rustdesk
docker-compose pull
docker-compose up -d
```

**二进制方式：**

```bash
# 停止服务
systemctl stop hbbs hbbr

# 下载新版本
cd /opt/rustdesk
wget https://github.com/rustdesk/rustdesk-server/releases/download/NEW_VERSION/rustdesk-server-linux-amd64.zip
unzip -o rustdesk-server-linux-amd64.zip

# 启动服务
systemctl start hbbs hbbr
```

---

## 8. 参考资料

| 资源 | 链接 |
|------|------|
| RustDesk 官网 | https://rustdesk.com |
| 官方文档 | https://rustdesk.com/docs/zh-cn/ |
| 服务端仓库 | https://github.com/rustdesk/rustdesk-server |
| 客户端仓库 | https://github.com/rustdesk/rustdesk |
| 官方 FAQ | https://github.com/rustdesk/rustdesk/wiki/FAQ |

---

## 附录：快速部署脚本

一键部署脚本（Docker 方式）：

```bash
#!/bin/bash

# RustDesk 一键部署脚本
# 使用方法: bash deploy.sh <服务器IP>

SERVER_IP=$1

if [ -z "$SERVER_IP" ]; then
    echo "请提供服务器 IP 地址"
    echo "用法: bash deploy.sh <服务器IP>"
    exit 1
fi

echo "======================================"
echo "RustDesk 服务端一键部署脚本"
echo "服务器 IP: $SERVER_IP"
echo "======================================"

# 安装 Docker
if ! command -v docker &> /dev/null; then
    echo "正在安装 Docker..."
    curl -fsSL https://get.docker.com | sh
    systemctl start docker
    systemctl enable docker
fi

# 安装 Docker Compose
if ! command -v docker-compose &> /dev/null; then
    echo "正在安装 Docker Compose..."
    curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
fi

# 创建目录
mkdir -p /opt/rustdesk/data
cd /opt/rustdesk

# 创建 docker-compose.yml
cat > docker-compose.yml << EOF
version: '3'

networks:
  rustdesk-net:
    external: false

services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    command: hbbs -r ${SERVER_IP}:21117
    ports:
      - 21115:21115
      - 21116:21116
      - 21116:21116/udp
      - 21118:21118
    volumes:
      - ./data:/root
    networks:
      - rustdesk-net
    depends_on:
      - hbbr
    restart: unless-stopped

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest
    command: hbbr
    ports:
      - 21117:21117
      - 21119:21119
    volumes:
      - ./data:/root
    networks:
      - rustdesk-net
    restart: unless-stopped
EOF

# 启动服务
echo "正在启动 RustDesk 服务..."
docker-compose up -d

# 等待服务启动
sleep 5

# 显示结果
echo ""
echo "======================================"
echo "部署完成！"
echo "======================================"
echo ""
echo "服务状态："
docker-compose ps
echo ""
echo "公钥 (Key)："
cat /opt/rustdesk/data/id_ed25519.pub
echo ""
echo "======================================"
echo "客户端配置信息："
echo "ID 服务器: $SERVER_IP"
echo "中继服务器: $SERVER_IP"
echo "Key: $(cat /opt/rustdesk/data/id_ed25519.pub)"
echo "======================================"
```

保存为 `deploy.sh`，使用方法：

```bash
chmod +x deploy.sh
./deploy.sh 你的服务器IP
```

---

> **文档版本**：1.0
> **最后更新**：2025年1月
> **适用版本**：RustDesk Server 1.1.x
