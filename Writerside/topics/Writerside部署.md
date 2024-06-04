# Writerside部署

将Writerside部署部署到GitHubPages上

## 官方资料

[Build and publish on GitHub](https://www.jetbrains.com/help/writerside/deploy-docs-to-github-pages.html)

## 操作方法

### 打开pages 设置界面

项目-settings-pages

![image_11.png](image_11.png)

点击 create you own

### 配置自动化流水线

配置文件名(随意命名需要以.yml结尾)
文件内容可根据官方资料复制或者使用该文档下方代码处复制

![image_12.png](image_12.png)

```yaml
# 该文件需要修改部分进行了备注
name: Build documentation

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

permissions:
  id-token: write
  pages: write

env:
  INSTANCE: 'Writerside/hi' #此处hi需要替换成项目ID
  ARTIFACT: 'webHelpHI2-all.zip'# webHelpXX2-all.zip 此处XX需要替换成项目ID 
  DOCKER_VERSION: '241.15989'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Build docs using Writerside Docker builder
        uses: JetBrains/writerside-github-action@v4
        with:
          instance: ${{ env.INSTANCE }}
          artifact: ${{ env.ARTIFACT }}
          docker-version: ${{ env.DOCKER_VERSION }}

      - name: Save artifact with build results
        uses: actions/upload-artifact@v4
        with:
          name: docs
          path: |
            artifacts/${{ env.ARTIFACT }}
            artifacts/report.json
          retention-days: 7
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: docs
          path: artifacts

      - name: Test documentation
        uses: JetBrains/writerside-checker-action@v1
        with:
          instance: ${{ env.INSTANCE }}
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: [ build, test ]
    runs-on: ubuntu-latest
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: docs

      - name: Unzip artifact
        run: unzip -O UTF-8 -qq '${{ env.ARTIFACT }}' -d dir

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Package and upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: dir

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```

### 保存该文件

![image_13.png](image_13.png)

### 检查Actions运行情况

- 黄色: 正在运行
- 绿色: 已完成
- 红色: 错误

![image_14.png](image_14.png)

### 正常情况下完成后会显示文件链接 打开即可查看你的文档

![image_15.png](image_15.png)

