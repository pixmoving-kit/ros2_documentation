---
translation_status: machine_translated
source: How-To-Guides/Cross-compilation.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="cross-compilation"></span>

# 交叉编译

那个... [cross_compile](https://github.com/ros-tooling/cross_compile) 工具不再被支持。

交叉汇编的替代办法是: [构建多平台的 Docker 图像](https://github.com/docker/buildx#building-multi-platform-images) 使用 `docker buildx`.
