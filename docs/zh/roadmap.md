# Roadmap
Kubespider旨在构建一个通用的下载编排系统，兼容各种资源平台和下载软件，兼容多种下载方式（请求触发，周期触发，更新触发），做资源下载最完美的统一解决方案。

PS：如果你想做某个特性，请在repo issue中说明。

## 待做特性
* 增加如何开发的文档。
* 实现chrome插件，自动在网页插入下载或者追剧按钮。
* Chrome插件在保存server地址时做健康检查。
* 支持豆瓣评分大于X，自动搜索资源下载。
* 支持下载优先级，在某个下载器长期无法下载时，切换另外一个下载器。
* 完善现有文档，提供中英版本。
* 完善现有代码，定义好资源提供器和下载提供器的API。
* 设计测试框架，确保各source provider/download provder正常工作。
* 增加卸载脚本。
* 参照 `https://reorx.com/blog/track-and-download-shows-automatically-with-sonarr/`，集成到kubespider中，成为一个source provider。
* 集成alist，基于webDAV下载各云盘资源，成为一个source provider。
* 添加如下文档：如何配置Aria2，如何触发下载各种类型文件。
* README特性表格化，更易阅读。

## 正在做特性
* 更好的文件分类。

## v0.2.0版本特性(2023/04/30)
* 国内加速下载，支持阿里云image registry。
* 所有配置文件json化，state存储也json化。
* 搭建中国大陆部署测试流水线。
* 配置文件位置归一，.kubespider改为.config，放入${HOME}/kubespider/.config。
* 支持迅雷download provider。

## v0.1.0版本特性(2023/01/30)
* Chrome插件支持右键触发下载。
* 搭建发布流水线。
* 发布第一个版本。

## 舍弃
* 发布Kubespidre chrome插件。(发布Chrome插件需要各项审核，比较麻烦)
