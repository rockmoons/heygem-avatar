# HeyGem 服务端部署指南（运维向）

> 供有服务器的用户参考，普通用户使用「安装数字人」自动部署即可。

---

## 硬件要求

| 项目 | 最低 | 推荐 |
|------|------|------|
| GPU | RTX 3060 12GB | RTX 4070+ 或 A4000 |
| 显存 | 8GB | 12GB+ |
| 内存 | 16GB | 32GB |
| 磁盘 | 100GB SSD | 200GB+ |
| 系统 | Ubuntu 22.04 | Ubuntu 22.04 |

---

## 部署步骤

```bash
# 1. 安装 Docker + NVIDIA Toolkit
curl -fsSL https://get.docker.com | sh
# + NVIDIA Container Toolkit（见 heygem-install.md）

# 2. 拉取镜像
docker pull guiji2025/fun-asr
docker pull guiji2025/fish-speech-ziming
docker pull guiji2025/duix.avatar

# 3. 启动
docker compose up -d

# 4. 配置公网访问（Nginx 反代）
# 或使用 heygem-api 社区封装（github.com/lihuithe/heygem-api）
```

---

## 使用 heygem-api 封装

社区封装提供了 HTTP API + 火山引擎 OSS 存储，可用于云端服务：

```
https://github.com/lihuithe/heygem-api
```

部署后暴露公网端点，配置到 config.json 的 `heygem.cloud.api_url` 即可使用。
