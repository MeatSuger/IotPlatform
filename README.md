# IoT Platform Monorepo

一个物联网平台多仓库工作区，包含 Go 后端、Vue 3 前端管理台以及 ESP32 设备固件。

## 目录概览

| 目录 | 说明 | 技术栈 |
|------|------|--------|
| `back/` | 后端服务 | Go 1.26 + Gin + Ent ORM + PostgreSQL + Redis + InfluxDB + MQTT |
| `font/` | Web 管理台 | Vue 3 + Vite + TypeScript + Element Plus + UnoCSS + Pinia（基于 Fantastic-admin） |
| `firmware/` | 设备固件 | ESP-IDF (CMake) — WiFi / MQTT / 传感器 |
| `db/` | 数据库参考 | SQL 导出文件（用户、设备、下行命令表结构参考） |

## 子仓库

- `back` → [iotPlatform_back](https://github.com/MeatSuger/iotPlatform_back)
- `font` → [IotPlatform_web](https://github.com/MeatSuger/IotPlatform_web)
- `firmware` → [IotPlatform_firmware](https://github.com/MeatSuger/IotPlatform_firmware)

## 快速开始

### 1) 克隆与初始化

```bash
git clone <repo-url>
cd <repo-folder>
git submodule update --init --recursive
```

### 2) 启动后端（Go）

```bash
cd back

# 拷贝并编辑配置文件
cp configs/config.yaml configs/config.local.yaml
# 按需修改 PostgreSQL / Redis / InfluxDB / MQTT 连接信息

# 安装依赖并运行（热重载）
make deps
make dev

# 或直接编译运行
make build
./build/iot-platform
```

详见 `back/README.md`。

### 3) 启动前端

```bash
cd font

# 安装依赖
pnpm install

# 启动开发服务器
pnpm dev

# 构建生产版本
pnpm build
```

环境变量在 `apps/core-element-plus/.env.development` 中配置。

详见 `font/README.md`。

### 4) 编译/烧录固件（ESP32）

```bash
cd firmware

# 设置 ESP-IDF 环境后
idf.py set-target esp32
idf.py build
idf.py -p /dev/ttyUSB0 flash
idf.py -p /dev/ttyUSB0 monitor
```

详见 `firmware/AGENTS.md`。

## 常见问题

- **外部依赖**：需要 PostgreSQL 16+、Redis 7+、InfluxDB 2.7+（可选 MQTT Broker）。可用 `back/deployments/` 下的 Docker Compose 快速启动。
- **端口占用**：后端默认 `8182`，前端默认 Vite 开发端口。如冲突请修改后端 `configs/config.yaml` 或前端 `vite.config.ts`。
- **CORS**：后端已配置 Gin CORS 中间件，生产环境请在 `configs/config.prod.yaml` 中设置 `cors.allowed-origins`。

## 贡献指南

- 提交前拉取更新：

```bash
git pull --rebase
git submodule update --remote
```

- 后端代码在 `back/internal/`（controller / service / repository / model / middleware / websocket），入口 `back/cmd/iot-platform/main.go`
- 前端页面在 `font/apps/core-element-plus/src/views/iot/`，API 封装在 `font/apps/core-element-plus/src/api/`
- 固件模块在 `firmware/main/`（app / wifi / mqtt / sensor / peripherals / token / net / core）
