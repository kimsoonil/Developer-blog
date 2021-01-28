---
layout: post
title:  "프론트엔드 상태관리 mobX vs redux"
categories: JavaScript
tags: React redux mobX
author: KSI
---

* content
{:toc}
### 프론트엔드 상태관리 mobX vs redux

  |mobX|redux|
  |----|---|
  |처리속도 빠름|많은 개발도구, 라이브러리<br /> [redux-saga](https://github.com/redux-saga/redux-saga)<br/> [redux-thunk](https://github.com/reduxjs/redux-thunk) |
  |코드가 적음|디버그 툴이 존재|
  |도입하기 편함|하나의 store를 가짐|
  |여러 개의 store를 가질 수 있음|store의 데이터는 불변성을 가짐|
  |유연함|대량의 데이터를 사용시 유리함|



<br />

mobX 와 redux는 다음과 같은 특징이 있고 각기 목적에 따라 사용성이 다르다고 할수 있습니다

redux : 코드는 복잡하고 도입하기 힘들지만 대량의 데이터 많은 사용자들의 처리하기 수월

mobX : 코드가 적고 속도가 더 빠르며 도입하기 편하지만 redux 보다 상대적으로 덜 큰 프로젝트에 도입하기 적절

 

 

참고

[ https://velog.io/@silverj-kim/Front-endReact-Mobx-vs-Redux-Redux-toolkit](https://velog.io/@silverj-kim/Front-endReact-Mobx-vs-Redux-Redux-toolkit)

[https://boxfoxs.tistory.com/383 ](https://boxfoxs.tistory.com/383)

[ https://woowabros.github.io/experience/2019/01/02/kimcj-react-mobx.html](https://woowabros.github.io/experience/2019/01/02/kimcj-react-mobx.html)

[ https://medium.com/@punkyoon/mobx를-사용하면-안되는-경우-a49d24b44580](https://medium.com/@punkyoon/mobx를-사용하면-안되는-경우-a49d24b44580)