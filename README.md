# IoT Platform Monorepo

一个包含后端（Spring Boot / Maven）、前端（Vite + Vue 3 / TS）以及设备固件（PlatformIO）的物联网平台多仓库工作区。

目录概览

- `back/`：Java 后端服务（Spring Boot + Maven）
- `font/`：Web 前端（Vite + Vue 3 + TypeScript）
- `firmware/`：设备固件（PlatformIO / Arduino 栈）

快速开始（Windows + PowerShell）

1) 克隆与初始化

```powershell
git clone <repo-url>
cd <repo-folder>
git submodule update --init --recursive
```

2) 启动后端

```powershell
cd .\back
mvn -v
mvn clean install -DskipTests
mvn spring-boot:run
```

配置位于 `back/src/main/resources/application*.yml`，可通过 `--spring.profiles.active=dev|prod` 切换环境。

3) 启动前端

```powershell
cd ..\font
node -v
npm install
npm run dev
```

本地开发默认运行在 Vite 开发端口，API 代理与环境变量见 `font/vite.config.ts` 与 `font/.env*`（如存在）。

4) 编译/烧录固件（可选）

```powershell
cd ..\firmware
pio --version
pio run
pio run -t upload
pio device monitor
```

PlatformIO 项目参数见 `firmware/platformio.ini`。

常见问题与提示

- 外部依赖：如需要 InfluxDB/Redis/MQTT，请先本地或容器启动并在后端 `application.yml` 中配置。
- CORS/接口地址：后端端口或路径调整后，请同步前端请求配置（`font/src/utils/request.ts`、`font/vite.config.ts`）。
- 端口占用：如开发端口被占用，调整 Vite 端口或后端 `server.port`。

子仓库文档

- 详见 `back/README.md`（后端部署与配置）
- 详见 `font/README.md`（前端开发与构建）
- 详见 `firmware/README.md`（设备固件与串口监视）

贡献指南

- 提交前拉取更新并保持依赖一致：

```powershell
git pull --rebase
git submodule update --remote
```

- 代码位置：后端 `back/src/main/java`，前端 `font/src`，固件 `firmware/src`。
- 建议新增：API 文档样例、统一启动脚本（PowerShell）与环境示例文件。
