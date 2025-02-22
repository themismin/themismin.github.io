---
title: 技术-tiktok 视频下载教程
tags:
  - tiktok
  - youtube
  - yt-dlp
  - 视频下载
categories:
  - IT
  - Hecker
abbrlink: 1f9f539b
date: 2025-02-22 11:43:50
---

### 视频下载至本地
```
// 单个视频下载至本地
yt-dlp -o "%(title)s.%(ext)s" "https://www.tiktok.com/@xxxxxx/video/111111111"

// 用户下的所有视频下载至本地
yt-dlp --extractor-args "tiktok:user_agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36" --no-warnings --download-archive downloaded.txt --concurrent-fragments 3 --embed-thumbnail --merge-output-format mp4 -o "%(uploader)s/%(title)s.%(ext)s" "https://www.tiktok.com/@xxxxxx"
```
