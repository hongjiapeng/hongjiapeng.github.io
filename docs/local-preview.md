# 本地预览指南

本文记录 JP Blog（Jekyll + Chirpy）在新设备上的本地预览方法。

## Windows

### 1. 安装 Ruby 3.4 + DevKit

使用 PowerShell 执行：

```powershell
winget install RubyInstallerTeam.RubyWithDevKit.3.4 `
  --accept-source-agreements `
  --accept-package-agreements
```

安装完成后，关闭并重新打开 PowerShell，让 Ruby 加入 PATH。

确认安装成功：

```powershell
ruby --version
bundle --version
```

如果提示找不到 `ruby`，先重新打开终端；也可以检查：

```powershell
where.exe ruby
```

### 2. 获取项目并切换到迁移分支

```powershell
git clone https://github.com/hongjiapeng/hongjiapeng.github.io.git
cd hongjiapeng.github.io
git switch codex/migrate-chirpy
```

如果迁移分支已经合并到 `master`，直接使用默认分支即可。

### 3. 初始化 Chirpy 静态资源

```powershell
git submodule update --init --recursive
```

### 4. 安装依赖

```powershell
bundle install
```

验证依赖是否完整：

```powershell
bundle check
```

### 5. 启动本地预览

```powershell
bundle exec jekyll serve --livereload
```

浏览器打开：

<http://127.0.0.1:4000/>

修改 Markdown、配置或模板后，Jekyll 会自动重新构建；浏览器会通过 LiveReload 自动刷新。按 `Ctrl+C` 停止服务。

## macOS / Linux

先安装 Ruby 3.4、Bundler 和 Git，然后在项目根目录执行：

```bash
git submodule update --init --recursive
bundle install
bundle exec jekyll serve --livereload
```

然后打开 <http://127.0.0.1:4000/>。

## 让手机或局域网设备访问

在运行博客的电脑上执行：

```powershell
bundle exec jekyll serve --livereload --host 0.0.0.0
```

然后在另一台设备上访问运行电脑的局域网 IP，例如：

```text
http://192.168.1.100:4000/
```

如果无法访问，需要允许 Ruby/Jekyll 通过 Windows 防火墙，并确认两台设备处于同一个局域网。

## 常见问题

### `bundle` 找不到

重新打开终端后执行：

```powershell
ruby --version
gem install bundler
bundle --version
```

### `assets/lib` 为空或资源加载失败

重新初始化子模块：

```powershell
git submodule sync --recursive
git submodule update --init --recursive
```

### 端口 4000 被占用

换一个端口启动：

```powershell
bundle exec jekyll serve --livereload --port 4001
```

然后打开 <http://127.0.0.1:4001/>。

### 构建失败

先清理 Jekyll 缓存，再重新构建：

```powershell
Remove-Item -Recurse -Force .jekyll-cache -ErrorAction SilentlyContinue
Remove-Item -Recurse -Force _site -ErrorAction SilentlyContinue
bundle exec jekyll build --trace
```
