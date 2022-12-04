---
title: "一个消息补偿队列引发的P0级生产事故"
subtitle: ""
date: 2022-12-04T23:42:10+08:00
author: "Medivh"
featuredImagePreview: "https://i.imgur.com/I4bnpaT.jpg"
images: ["https://i.imgur.com/I4bnpaT.jpg"]
keywords: "消息队列"
weight: 0

tags:
- Java
categories:
- 技术

hiddenFromHomePage: false
hiddenFromSearch: false
math:
  enable: false
lightgallery: false
seo:
  images: ["https://i.imgur.com/I4bnpaT.jpg"]

# See details front matter: https://fixit.lruihao.cn/theme-documentation-content/#front-matter
---

<!--more-->

<a href="https://imgur.com/I4bnpaT"><img src="https://i.imgur.com/I4bnpaT.jpg" title="source: imgur.com" /></a>

上周因为一个消息补偿队列引发的P0级生产事故，本以为可以要走人了，而且是没有补偿的那种...

这就是出问题的代码，各位大佬应该已经看出来问题出在哪了，这里我要解释一下，代码不是我写的

```java
@Override
  protected ConsumeResult consumeMessage(String topic, String tag, Message message) {
    ConsumeResult result = new ConsumeResult();
    try {
      String body = JSON.parse(new String(message.getBody(), StandardCharsets.UTF_8)).toString();
      // 发起支付通知
      NotifyStory notifyStory = JSONObject.parseObject(body, NotifyStory.class);
      paymentApplicationService.payNotify(notifyStory);
      result.setOk(true);
    } catch (Exception e) {
      LoggerUtil.error("PaymentNotificationRetryConsumer.consumeMessage 发生异常", e);
      result.setOk(false);
    }
    return result;
  }
  ```

  ---

  <script async src="https://comments.app/js/widget.js?3" data-comments-app-website="2h9SXzXx" data-limit="5" data-outlined="1" data-colorful="1" data-dark="1"></script>