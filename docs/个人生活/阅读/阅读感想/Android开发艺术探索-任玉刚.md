---
doc_type: weread-highlights-reviews
bookId: "23735061"
reviewCount: 1
noteCount: 10
author: 任玉刚
cover: https://wfqqreader-1252317822.image.myqcloud.com/cover/61/23735061/t7_23735061.jpg
readingStatus: 读过
progress: 4%
totalReadDay: 2
readingTime: 0小时32分钟
readingDate: 2020-09-14
isbn: 9787121269394
lastReadDate: 2020-09-14

---
# 元数据
> [!abstract] Android开发艺术探索
> - ![ Android开发艺术探索|200](https://wfqqreader-1252317822.image.myqcloud.com/cover/61/23735061/t7_23735061.jpg)
> - 书名： Android开发艺术探索
> - 作者： 任玉刚
> - 简介： 本书是一本Android进阶类书籍，采用理论、源码和实践相结合的方式来阐述高水准的Android应用开发要点。本书从三个方面来组织内容。第一，介绍Android开发者不容易掌握的一些知识点；第二，结合Android源代码和应用层开发过程，融会贯通，介绍一些比较深入的知识点；第三，介绍一些核心技术和Android的性能优化思想。
> - 出版时间： 2015-09-01 00:00:00
> - ISBN： 9787121269394
> - 分类： 计算机-编程设计
> - 出版社： 电子工业出版社
> - PC地址：https://weread.qq.com/web/reader/9d932320716a2b159d9b881

# 高亮划线

### 1.1 Activity的生命周期全面分析

> 📌 典型情况下 
> ⏱ 2020-09-14 08:53:22 ^23735061-7-485-490

> 📌 异常情况 
> ⏱ 2020-09-14 08:53:29 ^23735061-7-501-505

> 📌 从Activity是否在前台来说，onResume和onPause是配对的 
> ⏱ 2020-09-14 09:29:25 ^23735061-7-2952-2989

> 📌 onStart和onStop是从Activity是否可见这个角度来回调的 
> ⏱ 2020-09-14 09:31:09 ^23735061-7-3452-3488

> 📌 onResume和onPause是从Activity是否位于前台这个角度来回调的 
> ⏱ 2020-09-14 09:31:34 ^23735061-7-3490-3530

> 📌 在新Activity启动之前，桟顶的Activity需要先onPause后，新Activity才能启动 
> ⏱ 2020-09-14 09:34:07 ^23735061-7-4682-4733

> 📌 旧Activity先onPause，然后新Activity再启动 
> ⏱ 2020-09-14 09:35:56 ^23735061-7-5585-5617

> 📌 系统配置发生改变后，Activity会被销毁，其onPause、onStop、onDestroy均会被调用，同时由于Activity是在异常情况下终止的，系统会调用 
> ⏱ 2020-09-14 09:57:44 ^23735061-7-10500-10582

> 📌 onSaveInstanceState来保存当前Activity的状态。这个方法的调用时机是在onStop之前，它和onPause没有既定的时序关系 
> ⏱ 2020-09-14 09:58:20 ^23735061-7-10582-10656

> 📌 这个方法只会出现在Activity被异常终止的情况下，正常情况下系统不会回调这个方法 
> ⏱ 2020-09-14 10:01:09 ^23735061-7-10699-10741

# 读书笔记

## 1.1 Activity的生命周期全面分析

### 划线评论
> 📌 这里有一种特殊情况，如果新Activity采用了透明主题，那么当前Activity不会回调onStop  ^292104720-7klIGakHh
    - 💭 透明主题?
    - ⏱ 2020-09-14 09:26:09
   
# 本书评论

