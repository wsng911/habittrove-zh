# habittrove-zh Prompts

> 项目：dohsimpson/HabitTrove 汉化版（habittrove-zh）
> 技术栈：Next.js 14 + React + Tailwind CSS + Radix UI，next-intl 国际化，Jotai 状态管理

---

## 功能迭代

**1. 添加习惯完成提醒通知**
在 habittrove-zh 中添加浏览器推送通知功能。用户可以为每个习惯设置提醒时间，到时间后通过 Web Push API 发送通知。需要在 AddEditHabitModal 中添加提醒时间字段，后端使用 web-push 库发送通知。

**2. 添加习惯数据统计图表**
在 habittrove-zh 的 Dashboard 中添加习惯完成率统计图表，使用 recharts 展示过去 30 天每天的完成情况（折线图）和各习惯完成率对比（柱状图）。数据从现有的 HabitStreak 数据中聚合。

**3. 支持习惯模板导入**
在 habittrove-zh 中添加"从模板创建"功能，提供常见习惯模板（如：早起、运动、阅读、冥想等），用户可以一键导入预设的习惯配置，包括名称、频率、金币奖励和描述。

**4. 添加好友挑战功能**
在 habittrove-zh 中添加多用户挑战功能。用户可以创建挑战并邀请其他用户参与，挑战期间显示排行榜，按完成习惯数量排名，激励用户坚持打卡。

**5. 支持习惯完成照片记录**
在 habittrove-zh 中为习惯完成添加可选的照片记录功能。用户完成习惯时可以拍照或上传图片作为打卡凭证，照片保存在服务器并在日历视图中显示缩略图。

---

## Bug 修复

**6. 修复番茄钟计时器在后台标签页暂停的问题**
在 habittrove-zh 的 PomodoroTimer 组件中，当用户切换到其他标签页时，计时器会因浏览器节流而不准确。请使用 Web Worker 或 Service Worker 在后台精确计时，确保计时器在标签页不活跃时仍然准确。

**7. 修复愿望清单金币不足时仍可兑换的问题**
在 habittrove-zh 的 WishlistItem 组件中，当用户金币余额不足时，兑换按钮应该被禁用，但在某些竞态条件下仍可点击并扣除金币导致余额为负。在后端 actions 中添加原子性的余额检查。

**8. 修复习惯频率设置为"每周"时日历显示错误**
在 habittrove-zh 的 HabitCalendar 组件中，当习惯频率设置为每周特定天数时，日历上的完成标记有时会显示在错误的日期。检查 HabitCalendar 中的日期计算逻辑，确保时区处理正确。

**9. 修复移动端侧边栏遮挡内容的问题**
在 habittrove-zh 的移动端视图中，MobileNavDisplay 组件展开时会遮挡主内容区域，且点击内容区域无法关闭侧边栏。加个点击遮罩层关闭侧边栏的功能，并确保侧边栏使用正确的 z-index 层级。

**10. 修复数据备份文件包含敏感信息的问题**
在 habittrove-zh 的数据备份功能中，导出的备份文件包含了用户密码哈希等敏感信息。请修改备份逻辑，在导出时过滤掉密码字段，或对敏感字段进行加密处理。

---

## 重构

**11. 将 Jotai atoms 按功能模块拆分**
habittrove-zh 中所有 Jotai atoms 都定义在单个文件中，随着功能增加变得难以维护。请按功能模块拆分：`atoms/habits.ts`、`atoms/wishlist.ts`、`atoms/coins.ts`、`atoms/settings.ts`，并更新所有引用。

**12. 统一 Server Actions 的错误处理**
habittrove-zh 的各个 Server Actions 错误处理方式不一致，有些返回 null，有些抛出异常。请创建统一的 `ActionResult<T>` 类型和错误处理工具函数，所有 actions 返回 `{ data: T | null, error: string | null }`。

---

## 测试

**13. 为习惯 CRUD Server Actions 编写测试**
使用 Jest 为 habittrove-zh 的习惯相关 Server Actions（createHabit、updateHabit、deleteHabit、completeHabit）编写单元测试，使用临时文件系统模拟数据存储，覆盖正常流程和边界情况。

**14. 为金币计算逻辑编写单元测试**
为 habittrove-zh 中的金币奖励计算逻辑编写完整单元测试，覆盖：完成习惯获得金币、兑换愿望清单扣除金币、金币余额不足时的处理、连续打卡奖励倍数计算。

**15. 为 HabitCalendar 组件编写快照测试**
使用 React Testing Library 为 habittrove-zh 的 HabitCalendar 组件编写快照测试和交互测试，覆盖：不同月份的日历渲染、完成记录的显示、日期点击事件、跨月导航。

---

## 代码理解

**16. 解释 next-intl 的国际化实现方式**
在 habittrove-zh 中使用了 next-intl 实现多语言支持。解释语言文件的加载机制、`useTranslations` hook 的工作原理、语言切换如何触发页面重新渲染，以及如何在 Server Components 和 Client Components 中分别使用翻译。

**17. 解释 Jotai 与 Server Actions 的数据流**
在 habittrove-zh 中，前端使用 Jotai 管理状态，后端使用 Next.js Server Actions 处理数据。解释数据从 Server Action 返回后如何更新 Jotai atoms、乐观更新是如何实现的，以及 `jotai-hydrate` 组件的作用。

---

## DevOps

**18. 编写 GitHub Actions 自动构建流水线**
为 habittrove-zh 编写 `.github/workflows/docker-build.yml`，实现推送 main 分支时自动构建 Docker 镜像并推送到 Docker Hub，支持多架构（amd64/arm64），使用 npm 缓存加速构建。

**19. 编写健康检查和监控配置**
为 habittrove-zh 添加健康检查接口 `/api/health`，返回服务状态、数据目录可写性、内存使用情况。在 Dockerfile 中添加 HEALTHCHECK 指令，在 docker-compose.yml 中配置健康检查和自动重启。

**20. 编写数据迁移脚本**
为 habittrove-zh 编写数据迁移脚本 `scripts/migrate.js`，支持从旧版本数据格式迁移到新版本。脚本需要：备份原始数据、验证数据格式、执行迁移转换、验证迁移结果，并在迁移失败时自动回滚。
