---
layout: post
title: Notification 
description: "안드로이드 푸시 팝업"
modified: 2018-11-27
tags: [notification]
categories: [android,notification]
---

# 안드로이드 푸시 팝업

순서 

1. NotificationManager 얻어옴
2. 오레오 버전 이상이면 Channel 생성 아니면 패스
3. PendingIntent생성
4. NotificationCompat 생성
5. 빌드한 Notification을 notificationManager에 붙여주면 끝! 
6. Notification 클릭 시 핸들

1.NotificationManager 얻어옴
{% highlight ruby %}
val notificationManager: NotificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
{% endhighlight %}

2.오레오 버전 이상이면 Channel 생성 아니면 패스
{% highlight ruby %}
     if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                val channel = NotificationChannel(getString(R.string.app_name) + getString(R.string.priority_high), getString(R.string.app_name) + getString(R.string.priority_high), NotificationManager.IMPORTANCE_HIGH)
                channel.description = getString(R.string.app_name) + getString(R.string.priority_high)
                channel.enableLights(true)
                channel.lightColor = Color.BLUE
                channel.enableVibration(true)
                channel.vibrationPattern = longArrayOf(100, 200, 100, 200)
                channel.lockscreenVisibility = NotificationCompat.VISIBILITY_PUBLIC
                notificationManager.createNotificationChannel(channel)
            }
{% endhighlight %}

Channel 생성 시 IMPORTANCE 설정에 따라 알림이 다르다.
NotificationManager.IMPORTANCE_HIGH :소리 및 팝업 (헤드업 알림)
NotificationManager.IMPORTANCE_DEFAULT : 소리
NotificationManager.IMPORTANCE_LOW : 소리 없음    
NotificationManager.IMPORTANCE_MIN : 소리 및 시각적 알림 없음

3.PendingIntent생성
{% highlight ruby %}
   val intent = Intent(applicationContext, MainActivity::class.java)
            remoteMessage.data?.apply {
                intent.putExtra(JSON_LETTER, get(JSON_LETTER))
                intent.putExtra(JSON_ANSWER, get(JSON_ANSWER))
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)
                intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP)
            }
   val pendingIntent = PendingIntent.getActivity(applicationContext, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT)
{% endhighlight %}

알림 팝업을 누르게 되면 PendingIntent 에 실어 두었던 Intent 정보를 가져와서 사용할 수 있다.

4.NotificationCompat 생성
{% highlight ruby %}
   val builder = NotificationCompat.Builder(applicationContext, getString(R.string.app_name) + getString(R.string.priority_high))
                    .setSmallIcon(R.mipmap.ic_launcher)
                    .setContentTitle(title)
                    .setContentText(body)
                    .setDefaults(Notification.DEFAULT_VIBRATE)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)
                    .setAutoCancel(true)
                    .setContentIntent(pendingIntent)
                    .setVisibility(NotificationCompat.VISIBILITY_PUBLIC)
{% endhighlight %}

5.빌드한 Notification을 notificationManager에 붙여주면 끝! 

{% highlight ruby %}
notificationManager.notify(1, builder.build())
{% endhighlight %}

6.Notification 클릭 시 핸들
{% highlight ruby %}
  override fun onNewIntent(intent: Intent?) {
        super.onNewIntent(intent)
        intent?.apply {
            if (intent.extras.getString(JSON_ANSWER) != null) {
                val jsonLetter = intent.extras.getString(JSON_LETTER)
                val jsonAnswer = intent.extras.getString(JSON_ANSWER, EMPTY)
                if (supportFragmentManager.backStackEntryCount > 0) {
                    if (jsonLetter == null) {
                        val answer = Gson().fromJson(jsonAnswer, Answer::class.java)
                        openReAnswers(answer)
                    } else {
                        openReAnswers(jsonLetter, jsonAnswer)
                    }
                    return
                } else {
                    openHome()
                    if (jsonLetter == null) {
                        val answer = Gson().fromJson(jsonAnswer, Answer::class.java)
                        openReAnswers(answer)
                    } else {
                        openReAnswers(jsonLetter, jsonAnswer)
                    }
                    return
                }
            }
        }
    }
{% endhighlight %}

Notification 클릭 시 onNewIntent 메소드 호출 되는데 여기서 intent에 넣어 두었던 데이터를 사용하면 된다.