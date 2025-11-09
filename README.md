# IoT Platform

这是一个包含后端（Spring Boot / Maven）与前端（Vite + Vue 3）的物联网平台仓库。

仓库结构（节选）

- `back/` — 后端（Maven/Java）
  - `back/pom.xml`
  - `back/src/main/java/...` — Java 源码
- `font/` — 前端（Vite + Vue 3 / TypeScript）
  - `font/package.json`
  - `font/src/` — 前端源码

快速开始（Windows + PowerShell）

1. 克隆仓库并初始化子模块（如有）：

```powershell
git clone <repo-url>
cd <repo-folder>
git submodule update --init --recursive
```

2. 启动后端（进入 `back`）：

```powershell
cd .\back
# 构建
mvn clean install -DskipTests
# 运行（开发）
mvn spring-boot:run
```

默认配置文件位于 `back/src/main/resources/` 下的 `application*.yml`，可根据需要切换或覆盖环境变量。

3. 启动前端（进入 `font`）：

```powershell
cd ..\font
npm install
npm run dev
```

前端默认在 `font/src/main.ts` 启动，并使用 Vite 的开发服务器。

常见提示

- 如果后端使用 InfluxDB/Redis 等外部服务，请确保相应服务已启动，并在 `application.yml` 中配置好连接信息。
- 若修改了后端端口或 CORS 策略，请同步前端请求地址（通常在 `font/src/services` 或 `font/vite.config.ts` 中）。

贡献

- 提交代码前请拉取最新分支并更新子模块：

```powershell
git pull
git submodule update --remote
```

- 前端在 `font/src` 下，后端在 `back/src/main/java` 下。

- 把 README 扩展为包含 API 文档示例。
- 添加运行脚本（PowerShell 脚本）来一键启动前后端。
