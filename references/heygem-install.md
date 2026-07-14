# HeyGem 本地安装指南（含 Docker 全自动）

---

## Step 0 · 磁盘检测

安装前扫描所有盘符，找剩余 > 100GB 的盘：

```bash
# Linux
df -h --output=target,avail | tail -n +2

# Windows (Git Bash)
df -h | grep -E "^[A-Z]:"
```

```
检测结果：
  C 盘 45GB ❌（需 100GB+）
  D 盘 500GB ✅
  E 盘 200GB ✅
→ 自动选 D 盘，不占 C 盘空间
```

**告知用户：**「C盘空间不太够，帮你装到D盘了～」

---

## Step 1 · 检测 GPU

```bash
nvidia-smi
```

有显卡 → 显示型号+显存 → 「检测到 RTX 4070（12GB），完全够用 ✅」
无显卡 → 「未检测到 NVIDIA 显卡。数字人需要 NVIDIA 显卡才能本地运行。建议用云端模式，或换一台有显卡的电脑」

---

## Step 2 · 安装 Docker

用户确认后才开始。先提示：「需要约 70GB 磁盘 + 约 15 分钟下载。确认开始？」

### Linux（全自动）

```bash
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER

# NVIDIA Container Toolkit
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -fsSL https://nvidia.github.io/libnvidia-container-toolkit/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
curl -s -L https://nvidia.github.io/libnvidia-container-toolkit/$distribution/libnvidia-container.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt update && sudo apt install -y nvidia-container-toolkit
sudo systemctl restart docker

# 配置 data-root 到目标盘（如 D 盘不够用 C 盘的情况）
sudo mkdir -p /data/docker
sudo tee /etc/docker/daemon.json <<< '{"data-root":"/data/docker"}'
sudo systemctl restart docker
```

### Windows

```bash
# 先检查 WSL2
wsl --version 2>/dev/null || wsl --install

# 尝试自动安装 Docker Desktop
winget install Docker.DockerDesktop --accept-package-agreements --silent
```

如果 winget 失败：
```
「自动安装卡住了。请手动下载 Docker Desktop：
  https://www.docker.com/products/docker-desktop/
  装好后说「装好了」继续」

Docker Desktop 设置中把镜像目录改到 D 盘：
  Settings → Resources → Advanced → Disk image location → D:\Docker
```

WSL2 安装后可能需重启，重启后用户说「继续」。

---

## Step 3 · 拉取镜像 + 启动

```bash
docker pull guiji2025/fun-asr
docker pull guiji2025/fish-speech-ziming
docker pull guiji2025/duix.avatar

# 启动
cd /path/to/heygem-deploy
docker compose up -d
```

进度：`📥 下载数字人模型（首次约 70GB）▓░░░░░ 15%`

---

## Step 4 · 验证

```bash
curl http://127.0.0.1:8383/health
# 返回 OK → ✅
```

```
✅ 数字人服务已就绪！
   现在可以说「体验一下」或「生成数字人视频」了
```

---

## 错误处理

| 出错 | 处理 |
|------|------|
| Docker 安装失败 | 「Docker 安装卡住了。手动装：[链接]，装好说「装好了」」 |
| nvidia-smi 无输出 | 「没检测到 NVIDIA 显卡。换云端模式？」 |
| 磁盘不足 | 「所有盘都不够 100GB。需要清理空间或外接硬盘」 |
| 镜像拉取超时 | 重试，建议使用国内 Docker 镜像源 |
| 端口冲突 | 换端口，修改 docker-compose.yml |
