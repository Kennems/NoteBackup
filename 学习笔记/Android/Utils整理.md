---
title : 'Utils整理'
date : 2025-04-29T10:30:13+08:00
lastmod: 2024-04-29T10:20:13+08:00
description : "Android学习笔记" 
categories : ["Android学习笔记"]
tags : ["Android"]
---

# Utils整理

## AppSizeUtils

```kotlin
import android.app.usage.StorageStatsManager
import android.content.Context
import android.content.pm.ApplicationInfo
import android.os.Build
import android.os.storage.StorageManager
import android.util.Log
import androidx.annotation.RequiresApi
import java.io.File
import java.util.Locale

object AppSizeUtils {

    /**
     * 获取当前应用的 APK 安装包大小（单位：字节）
     */
    fun getApkSize(context: Context): Long {
        val appInfo: ApplicationInfo =
            context.packageManager.getApplicationInfo(context.packageName, 0)
        val apkFile = File(appInfo.sourceDir)
        val size = apkFile.length()
        Log.d("Kennem", "APK Size = ${size / (1024.0 * 1024.0)} MB")
        return size
    }

    /**
     * 获取当前应用安装后占用的存储空间总大小（包含 code、data、cache）
     * 仅适用于 Android 8.0 及以上
     */
    @RequiresApi(Build.VERSION_CODES.O)
    fun getAppStorageSize(context: Context): Long {
        val storageStatsManager =
            context.getSystemService(Context.STORAGE_STATS_SERVICE) as StorageStatsManager
        val storageManager = context.getSystemService(Context.STORAGE_SERVICE) as StorageManager
        val appInfo = context.applicationInfo
        val uuid = storageManager.getUuidForPath(File(appInfo.sourceDir))

        val stats = storageStatsManager.queryStatsForUid(uuid, appInfo.uid)
        val codeSize = stats.appBytes
        val dataSize = stats.dataBytes
        val cacheSize = stats.cacheBytes

        val totalSize = codeSize + dataSize + cacheSize
        Log.d("Kennem", "Installed Size = ${totalSize / (1024.0 * 1024.0)} MB")
        return totalSize
    }

    /**
     * 将字节大小格式化为 MB 字符串，保留两位小数
     */
    fun formatSizeToMB(bytes: Long): String {
        return String.format(Locale.US, "%.2f MB", bytes / (1024.0 * 1024.0))
    }
}
```

## ToastUtil

```kotlin
import android.content.Context
import android.widget.Toast

object ToastUtil {
    private var toast: Toast? = null

    fun show(context: Context, message: String, duration: Int = Toast.LENGTH_SHORT) {
        toast?.cancel()
        toast = Toast.makeText(context, message, duration)
        toast?.show()
    }
}
```

## NotificationUtil

```kotlin
import android.app.NotificationChannel
import android.app.NotificationManager
import android.app.PendingIntent
import android.content.Context
import android.content.Intent
import android.graphics.Bitmap
import android.graphics.BitmapFactory
import android.media.RingtoneManager
import android.net.Uri
import android.os.Build
import android.util.Log
import androidx.core.app.NotificationCompat
import androidx.core.app.Person
import com.guan.music_recommendation_tf.R
import com.guan.music_recommendation_tf.activity.NotificationActivity
import com.guan.music_recommendation_tf.activity.WebViewActivity

object NotificationUtil {

    const val TAG = "NotificationUtil"

    /**
     * 获取系统通知管理器
     */
    private fun getNotificationManager(context: Context): NotificationManager =
        context.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager

    /**
     * 获取默认的 PendingIntent（跳转到 NotificationActivity）
     */
    private fun getDefaultPendingIntent(context: Context): PendingIntent {
        val intent = Intent(context, NotificationActivity::class.java)
        return PendingIntent.getActivity(context, 0, intent, PendingIntent.FLAG_IMMUTABLE)
    }

    /**
     * 初始化所有通知渠道
     */
    fun initNotificationChannels(context: Context) {
        val manager = getNotificationManager(context)
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            // 普通通知渠道（低优先级）
            val normalChannel = NotificationChannel(
                "normal",
                "Normal Notifications",
                NotificationManager.IMPORTANCE_DEFAULT
            )
            // 重要通知渠道（高优先级）
            val importantChannel = NotificationChannel(
                "important",
                "Important Notifications",
                NotificationManager.IMPORTANCE_HIGH
            )
            // 头顶通知渠道（悬浮通知）
            val headsUpChannel = NotificationChannel(
                "heads_up_channel",
                "Heads-up Notifications",
                NotificationManager.IMPORTANCE_HIGH
            ).apply {
                enableVibration(true)
                vibrationPattern = longArrayOf(0, 250, 250, 250)
                setSound(RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION), null)
            }
            // 声音与震动通知渠道
            val soundVibrateChannel = NotificationChannel(
                "sound_vibrate_channel",
                "Sound & Vibrate Channel",
                NotificationManager.IMPORTANCE_HIGH
            ).apply {
                enableVibration(true)
                vibrationPattern = longArrayOf(0, 250, 250, 250)
                setSound(RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION), null)
            }

            listOf(normalChannel, importantChannel, headsUpChannel, soundVibrateChannel)
                .forEach { manager.createNotificationChannel(it) }
        }
    }

    /**
     * 发送基本通知，所有参数均可外部指定
     */
    fun sendBasicNotification(
        context: Context,
        contentTitle: String = "基本通知",
        contentText: String = "这是一个基本通知",
        smallIcon: Int = R.drawable.boat
    ) {
        val manager = getNotificationManager(context)
        val pendingIntent = getDefaultPendingIntent(context)
        val notification = NotificationCompat.Builder(context, "normal")
            .setContentTitle(contentTitle)
            .setContentText(contentText)
            .setSmallIcon(smallIcon)
            .setContentIntent(pendingIntent)
            .build()
        manager.notify(101, notification)
    }

    /**
     * 发送 BigTextStyle（长文本）通知
     * 外部可指定 bigText、contentText、smallIcon 以及大图 bitmap
     */
    fun sendBigTextNotification(
        context: Context,
        bigText: String = "BigTextStyle",
        contentTitle: String = "通知标题",
        contentText: String = "点击展开查看更多文本内容",
        smallIcon: Int = R.drawable.music_logo2,
        url: String,
        bitmap: Bitmap
    ) {
        Log.d(TAG, "sendBigTextNotification: $url")
        val manager = getNotificationManager(context)
        val contentIntent = PendingIntent.getActivity(
            context,
            0,
            Intent(context, WebViewActivity::class.java).putExtra("url", url),
            PendingIntent.FLAG_UPDATE_CURRENT or PendingIntent.FLAG_IMMUTABLE
        )
        val notification = NotificationCompat.Builder(context, "normal")
            .setSmallIcon(smallIcon)
            .setLargeIcon(bitmap)
            .setContentTitle(contentTitle)
            .setContentText(contentText)
            .setStyle(NotificationCompat.BigTextStyle().bigText(bigText))
            .setPriority(NotificationCompat.PRIORITY_HIGH)
            .setContentIntent(contentIntent)
            .setAutoCancel(true)
            .build()
        manager.notify(102, notification)
    }

    /**
     * 发送 BigPictureStyle（大图）通知
     * 外部可指定标题、文本、摘要、图片和图标
     */
    fun sendBigPictureNotification(
        context: Context,
        contentTitle: String = "BigPictureStyle通知",
        contentText: String = "点击查看大图",
        summaryText: String = "图片通知摘要",
        smallIcon: Int = R.drawable.boat,
        // 若外部未传入大图，则使用资源文件默认解码出的图片
        largeIcon: Bitmap? = BitmapFactory.decodeResource(context.resources, smallIcon)
    ) {
        val manager = getNotificationManager(context)
        val pendingIntent = getDefaultPendingIntent(context)
        val notification = NotificationCompat.Builder(context, "important")
            .setContentTitle(contentTitle)
            .setContentText(contentText)
            .setSmallIcon(smallIcon)
            .setStyle(
                NotificationCompat.BigPictureStyle()
                    .bigPicture(largeIcon)
                    .setSummaryText(summaryText)
            )
            .setLargeIcon(largeIcon)
            .setContentIntent(pendingIntent)
            .build()
        manager.notify(103, notification)
    }

    /**
     * 发送 InboxStyle（多行消息）通知
     * 外部可指定标题、文本、图标和所有的消息行
     */
    fun sendInboxStyleNotification(
        context: Context,
        contentTitle: String = "InboxStyle通知",
        contentText: String = "滑动展开查看更多内容",
        smallIcon: Int = R.drawable.boat,
        lines: List<String> = listOf(
            "第一条消息内容",
            "第二条消息内容",
            "第三条消息内容",
            "第四条消息内容",
            "第五条消息内容"
        )
    ) {
        val manager = getNotificationManager(context)
        val pendingIntent = getDefaultPendingIntent(context)

        val inboxStyle = NotificationCompat.InboxStyle()
        inboxStyle.setBigContentTitle(contentTitle)
        lines.forEach { inboxStyle.addLine(it) }

        val notification = NotificationCompat.Builder(context, "normal")
            .setContentTitle(contentTitle)
            .setContentText(contentText)
            .setSmallIcon(smallIcon)
            .setStyle(inboxStyle)
            .setContentIntent(pendingIntent)
            .build()
        manager.notify(104, notification)
    }


    /**
     * 发送 MessagingStyle（对话消息）通知
     * 外部可指定会话标题、图标、自己的名称及对话消息（每条消息：消息内容、时间戳和发送者名称）
     */
    fun sendMessagingStyleNotification(
        context: Context,
        conversationTitle: String = "群聊消息",
        smallIcon: Int = R.drawable.boat,
        selfName: String = "自己",
        messages: List<Triple<String, Long, String>> = listOf(
            Triple("早上好，各位！", System.currentTimeMillis() - 1000 * 60, "张三"),
            Triple("大家注意交通安全！", System.currentTimeMillis(), "李四")
        )
    ) {
        val manager = getNotificationManager(context)
        val pendingIntent = getDefaultPendingIntent(context)
        val self = Person.Builder().setName(selfName).build()
        val messagingStyle = NotificationCompat.MessagingStyle(self)
            .setConversationTitle(conversationTitle)
        messages.forEach { (msg, timestamp, senderName) ->
            val sender = Person.Builder().setName(senderName).build()
            messagingStyle.addMessage(msg, timestamp, sender)
        }
        val notification = NotificationCompat.Builder(context, "important")
            .setSmallIcon(smallIcon)
            .setStyle(messagingStyle)
            .setContentIntent(pendingIntent)
            .build()
        manager.notify(105, notification)
    }

    /**
     * 发送头顶（悬浮）通知
     * 外部可指定标题、文本、图标、声音等参数
     */
    fun sendHeadsUpNotification(
        context: Context,
        contentTitle: String = "头顶通知",
        contentText: String = "这是一条悬浮在屏幕上方的紧急通知",
        smallIcon: Int = R.drawable.boat,
        soundUri: Uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)
    ) {
        val manager = getNotificationManager(context)
        val pendingIntent = getDefaultPendingIntent(context)
        val fullScreenIntent = PendingIntent.getActivity(
            context,
            0,
            Intent(context, NotificationActivity::class.java),
            PendingIntent.FLAG_IMMUTABLE
        )
        val notification = NotificationCompat.Builder(context, "heads_up_channel")
            .setContentTitle(contentTitle)
            .setContentText(contentText)
            .setSmallIcon(smallIcon)
            .setPriority(NotificationCompat.PRIORITY_HIGH)
            .setContentIntent(pendingIntent)
            .setSound(soundUri)
            .setVibrate(longArrayOf(0, 250, 250, 250))
            .setAutoCancel(true)
            .apply { setFullScreenIntent(fullScreenIntent, true) }
            .build()
        manager.notify(200, notification)
    }

    /**
     * 发送带声音和震动的通知
     * 外部可指定标题、文本、图标、声音等参数
     */
    fun sendSoundVibrateNotification(
        context: Context,
        contentTitle: String = "声音与震动测试",
        contentText: String = "测试声音、震动以及悬浮通知效果",
        smallIcon: Int = R.drawable.boat,
        soundUri: Uri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)
    ) {
        val manager = getNotificationManager(context)
        val pendingIntent = getDefaultPendingIntent(context)
        val fullScreenIntent = PendingIntent.getActivity(
            context,
            0,
            Intent(context, NotificationActivity::class.java),
            PendingIntent.FLAG_IMMUTABLE
        )
        val notification = NotificationCompat.Builder(context, "sound_vibrate_channel")
            .setContentTitle(contentTitle)
            .setContentText(contentText)
            .setSmallIcon(smallIcon)
            .setContentIntent(pendingIntent)
            .setFullScreenIntent(fullScreenIntent, true)
            .setPriority(NotificationCompat.PRIORITY_HIGH)
            .setAutoCancel(true)
            .setSound(soundUri)
            .setVibrate(longArrayOf(0, 250, 250, 250))
            .build()
        manager.notify(300, notification)
    }
}
```

