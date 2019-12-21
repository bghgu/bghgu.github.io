---
layout: post
title: "Java Annotation"
description: "Java Annotation"
date: 2019-04-05
tags: [자바]
comments: true
share: true
---

#### Annotation
* 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종이다.
* @기호를 붙여서 사용한다.
* JDK 1.5 버전 이상에서 사용할 수 있다.
* Compiletime, Runtime시에 해석될 수 있다.
* 클래스 파일에 포함되어 컴파일러에 의해 생성된 후 JVM에 포함되어 작동한다.
* @Target으로 어노테이션이 적용할 위치를 선택한다.
* @Retention으로 컴파일러가 어노테이션을 다루는 방법을 설정한다.