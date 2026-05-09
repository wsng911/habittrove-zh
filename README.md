# HabitTrove

## 功能特性

- 习惯与任务管理
- 完成习惯获得金币奖励
- 愿望清单兑换系统
- 番茄钟计时器
- 习惯日历与连续打卡统计
- 多语言支持（默认中文）

## 快速部署

```bash
docker run -d \
  -p 3000:3000 \
  -v $(pwd)/data:/app/data \
  --name habittrove-zh \
  wsng911/habittrove-zh:latest
```

访问 `http://localhost:3000`
