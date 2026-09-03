# 🐎 sing-box Mustang

基于 [reF1nd/sing-box](https://github.com/reF1nd/sing-box) 的自定义构建，支持 DNS 轮询策略。

## 特性

- **DNS 轮询策略**：支持在 DNS 组中使用 `round_robin` 策略
- **多平台支持**：Windows amd64、Linux arm64、Android SFA
- **自动化构建**：通过 GitHub Actions 自动构建和发布
- **自定义签名**：使用固定的 release keystore 进行 APK 签名

## 支持的平台

| 平台 | 架构 | 输出文件 |
|------|------|----------|
| Windows | amd64 | `sing-box-{version}-windows-amd64.zip` |
| Linux | arm64 | `sing-box-{version}-linux-arm64.tar.gz` |
| Android | arm64-v8a | `SFA-{version}-arm64-v8a.apk` |

## 如何构建

### 方法 1：GitHub Web UI

1. 前往 [Actions](https://github.com/Joelincn/sing-box-releases/actions)
2. 选择 "Build sing-box for daily use"
3. 点击 "Run workflow"
4. 填写参数：
   - **version**: 例如 `1.14.0-Mustang.1`
   - **commit_hash**: （可选）指定 commit hash
   - **build_windows**: 构建 Windows amd64（默认: true）
   - **build_linux_arm64**: 构建 Linux arm64（默认: true）
   - **build_android**: 构建 Android SFA（默认: true）

### 方法 2：GitHub CLI

```bash
gh workflow run build-daily-use.yml \
  --repo Joelincn/sing-box-releases \
  -f version=1.14.0-Mustang.1 \
  -f build_windows=true \
  -f build_linux_arm64=true \
  -f build_android=true
```

## 构建环境

- **Go 版本**: 1.26.7
- **Build Tags**: 使用官方标签 `release/DEFAULT_BUILD_TAGS` 和 `release/DEFAULT_BUILD_TAGS_WINDOWS`
- **Android NDK**: r28
- **Java**: 17 (Temurin)
- **Gradle**: 使用 Gradle cache 加速构建

## DNS 轮询策略

本构建包含 DNS 轮询组策略功能：

```json
{
  "dns": {
    "servers": [
      {
        "type": "group",
        "tag": "dns-group",
        "strategy": "round_robin",
        "servers": ["dns-a", "dns-b", "dns-c"]
      }
    ]
  }
}
```

**两种策略可用**：
- `concurrent`（默认）：并发查询所有服务器，返回最快响应
- `round_robin`：按轮询顺序查询服务器，支持自动故障转移

## 版本号说明

版本号格式：`{主版本}-Mustang.{修订号}`

- **主版本**：跟随 reF1nd/sing-box 的版本号（如 `1.14.0`）
- **Mustang**：Mustang Build 标识
- **修订号**：自定义构建的修订版本

示例：`1.14.0-Mustang.1`

## 同步上游

```bash
git remote add upstream https://github.com/reF1nd/sing-box.git
git fetch upstream
git merge upstream/main
```

## 相关仓库

- [Joelincn/sing-box](https://github.com/Joelincn/sing-box) - 包含 DNS 轮询策略的 fork
- [reF1nd/sing-box](https://github.com/reF1nd/sing-box) - 上游仓库

## 许可证

参见 [LICENSE](LICENSE) 文件。
