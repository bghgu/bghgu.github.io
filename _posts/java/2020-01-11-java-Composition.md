---
layout: post
title:  "Java 상속과 합성"
date:   2020-01-11 19:42:00
author: bghgu
categories: JAVA
tags: [JAVA]
---

#### Composition 합성
* 클래스를 상속하는 대신 기존의 클래스의 인스턴스를 참조하느 private 필드를 서브 클래스로 만들고자 했던 클래스에 만든다.
* 새 클래스의 각 인스턴스 메소드에서는 기존 클래스에 포함된 함수들을 호출하여 결과를 반환해준다. 이것을 포워딩(forwarding)이라고 한다.
* 이렇게 함으로서 새 클래스는 기존 클래스의 내부 구현에 종속되지 않으며, 새로운 메소드를 추가하더라도 새 클래스에는 영향을 주지 않는다.
* 이러한 클래스를 wrapper class 라고 한다.

#### 상속의 안전함과 위험함
* 안전함
    * 동일한 프로그래머가 서브 클래스와 슈퍼 클래스의 구현을 관장하는, 같은 패키지 내에서 상속을 하는 것은 안전하다.
* 위험함
    * 서로 다른 패키지에 있으며, 확장을 목적으로 설계되지 않은 일반적인 클래스를 상속받는 것은 위험하다.
* 상속은 캡슐화(encapsulation)을 위배한다.
* 올바른 동작을 위해 서브 클래스는 자신의 수퍼 클래스가 구현하는 상세 내역에 의존하게 된다.
* 이 때 수퍼 클래스의 구현 내용은 소프트웨어 배포판이 바뀌면서 변경될 수 있다.
* 이렇게 되면 서스 클래스의 코드를 그냥 사용하게 되면 제대로 동작하지 않을 가능성이 높으며, 수퍼 클래스의 변화를 항상 감지하고, 맞춰 진화해야 한다.
* 차후 배포판에서 수퍼 클래스에 새로운 메소드가 추가되면 서브 클래스가 허약하게 되는 원인이 될 수 있다.
* 추후 배포판의 수퍼 클래스에 새 메소드를 추가할 때 그 메소드와 signature는 동일하고, return type이 다른 메소드의 함수가 서브 클래스에 이미 있다면, 잘못된 오버라이딩으로 서브 클래스에서 컴파일 에러가 생긴다.
* 반대로, 서브 클래스의 새 함수와 같은 signature와 return type을 가진 함수를 수퍼 클래스에서 정의한다면, 잘못된 오버라이딩을 한 셈이 된다.
* 클래스의 내부 구현을 쓸데 없이 노출시킬수도 있다.
* 그렇게 되면, 외부 API와 내부 구현이 밀접하게 연계되어 클래스의 성능을 제한하게 된다.
* 심각하게는 클라이언트가 내부 메소드나 필드를 직접 접근할 수도 있다.
* 잘못된 설계와 구현일수록 더욱 심각해진다.
* 더 심각한 것은, 클라이언트가 수퍼 클래스를 직접 수정하여 서브 클래스의 불변성을 저해할 수 있다.

#### 참고
* http://www.darkkaiser.com/2007/07/16/%EC%83%81%EC%86%8D%EA%B3%BC-%ED%95%A9%EC%84%B1/
* https://aroundck.tistory.com/617#recentEntries