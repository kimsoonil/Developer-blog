---
layout: post
title:  "React Kakao login"
categories: JavaScript
tags: React Kakao login
author: KSI
---

* content
{:toc}

 [react-kakao-login](https://github.com/wonism/react-kakao-login) 해당 라이브러리를 적용하였습니다.

 [developers.kakao](https://developers.kakao.com/console/app) 에서 앱을 만들고 App Keys 추출 key중 JavaScript key 사용




![kakao](https://kimsoonil.github.io/img/kakao_appkey.png)
Kakao Login → Activation Settings 활성화, Redirect URI url 등록

 해당 프로젝트에서 npm install react-kakao-login  or yarn add react-kakao-login 추가

해당 프로젝트에 ``npm install react-kakao-login`` or ``yarn add react-kakao-login`` 추가

### function 


kakao.js

``` js
  return(
  <KakaoLogin
    token={token}
    onSuccess={console.log}
    onFail={console.error}
    onLogout={console.info}
  />

```
git에서 나와있는 코드는 위와 같다 이 코드에서 필요한 것만 내 코드에 적용하였다.

``` js
<KakaoLogin
  token={token}
  onSuccess={responseKakao}
  onFail={console.error}
  onLogout={console.info}
  useLoginForm
  render={({ onClick }) => {
    return (
      <button className={classes.kakaobtn} onClick={(e) => {
        e.preventDefault();
        onClick();
      }}>
        <Icon icon={kakaotalkIcon} width="26" height="26" />
      </button>
    );
  }}
/>
```
response

``` js
profile:{
  connected_at: "2021-01-14T08:38:17Z"
  id: 1595444908
  kakao_account:
  email: "slskdkfdl@naver.com"
  email_needs_agreement: false
  has_email: true
  is_email_valid: true
  is_email_verified: tr
  ue
  
  profile:{
    nickname: "김순일"
    profile_image_url: "http://k.kakaocdn.net/dn/c4bGi6/btqMvqZJWma/MeeLd8DhpsNIivRINNVA80/img_640x640.jpg"
    thumbnail_image_url: "http://k.kakaocdn.net/dn/c4bGi6/btqMvqZJWma/MeeLd8DhpsNIivRINNVA80/img_110x110.jpg"
    profile_needs_agreement: false
  }
  properties: {
    nickname: "김순일", 
    profile_image: "http://k.kakaocdn.net/dn/c4bGi6/btqMvqZJWma/MeeLd8DhpsNIivRINNVA80/img_640x640.jpg", 
    thumbnail_image: "http://k.kakaocdn.net/dn/c4bGi6/btqMvqZJWma/MeeLd8DhpsNIivRINNVA80/img_110x110.jpg"
  }
}

response:{
  access_token: "NmErbZVl9t5br4h1bgxm0iExiZQtJ7nZS1isOwopcNEAAAF3BGdmmg"
  expires_in: 7199
  refresh_token: "dvwQ8xUxi11kPC33Q6WiCrKIPwuKFk2eSSckUAopcNEAAAF3BGdmmA"
  refresh_token_expires_in: 5183999
  scope: "account_email profile"
  token_type: "bearer"
}
```

Logout

``` js
Kakao.Auth.logout()
```

