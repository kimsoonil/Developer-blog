---
layout: post
title:  "React Google login"
categories: JavaScript
tags: React Google login
author: KSI
---

* content
{:toc}
https://github.com/anthonyjgrove/react-google-login 해당 라이브러리를 적용하였습니다.

https://console.cloud.google.com/에서 앱을 만들고 API 개요로 이동 → 사용자 인증 정보 → 사용자 인증 정보 만들기 → 0Auth 클라이언트 ID 으로 클라이언트 ID 추출



![google_clientId](https://kimsoonil.github.io/img/google_clientId.png)

해당 프로젝트에  ``npm install react-google-login``  or ``yarn add react-google-login`` 추가

### function 

``` js
import React from 'react';
import ReactDOM from 'react-dom';
import GoogleLogin from 'react-google-login';
// or
import { GoogleLogin } from 'react-google-login';


const responseGoogle = (response) => {
  console.log(response);
}

ReactDOM.render(
  <GoogleLogin
    clientId="658977310896-knrl3gka66fldh83dao2rhgbblmd4un9.apps.googleusercontent.com"
    buttonText="Login"
    onSuccess={responseGoogle}
    onFailure={responseGoogle}
    cookiePolicy={'single_host_origin'}
  />,
  document.getElementById('googleButton')
);
```
git에서 나와있는 코드는 위와 같다 이 코드에서 필요한 것만 내 코드에 적용하였다.

google.js

``` js
 const responseGoogle = (response) =>{ 
        console.log(response)
    }
    
 <GoogleLogin
    clientId={clientId}
    onSuccess={responseGoogle}
    onFailure={response => console.log(response)}
    render={renderProps => (
      <button className={classes.googleBtn} onClick={renderProps.onClick}>
      <Icon path={mdiGooglePlus}
        title="User Profile"
        size={1}
        color={"#fff"}
        />
      </button>
    )}
  />

```

response

``` js
Bc: {
    access_token: "ya29.a0AfH6SMDox9Gz26d5g_GiTwW-hzxIMyYkpmbIR498Ukz9fSSD4Jd5ugTaPZZ49b73K9rBWm4eqpIyf1262KyqKd1k88kUiRqJ8qTvERzyFf-B9x9ybcSpHuqiIlBQd2bEMuC_FC4IyBtX00qH0muSRcIHq1IvrYrFO7BxsugLzsM"
    expires_at: 1610517650513
    expires_in: 3599
    first_issued_at: 1610514051513
    id_token: "eyJhbGciOiJSUzI1NiIsImtpZCI6Ijc4M2VjMDMxYzU5ZTExZjI1N2QwZWMxNTcxNGVmNjA3Y2U2YTJhNmYiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJhY2NvdW50cy5nb29nbGUuY29tIiwiYXpwIjoiNzY3NDQzNTgzNTI2LWJiZmpoMnZrOTE5NnJuYWFrY3BndnZqcDR2dW5pdmp0LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwiYXVkIjoiNzY3NDQzNTgzNTI2LWJiZmpoMnZrOTE5NnJuYWFrY3BndnZqcDR2dW5pdmp0LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwic3ViIjoiMTE2MzkyNzQ1MDg2MDQxMDk0MTYyIiwiaGQiOiJzb2Z0d2FyZWlubGlmZS5jb20iLCJlbWFpbCI6InN1bmlsLmtpbUBzb2Z0d2FyZWlubGlmZS5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYXRfaGFzaCI6IlozcnJnTXN2cmFIUXl6MG9XRkRyVXciLCJuYW1lIjoi6rmA7Iic7J28IiwicGljdHVyZSI6Imh0dHBzOi8vbGg2Lmdvb2dsZXVzZXJjb250ZW50LmNvbS8tMjdKY3hrNjAxaWsvQUFBQUFBQUFBQUkvQUFBQUFBQUFBQUEvQU1adXVjbHpZV19sSDhmdmRaVjBtTkx5aHQ2blh3dGFRZy9zOTYtYy9waG90by5qcGciLCJnaXZlbl9uYW1lIjoi7Iic7J28IiwiZmFtaWx5X25hbWUiOiLquYAiLCJsb2NhbGUiOiJrbyIsImlhdCI6MTYxMDUxNDA1MSwiZXhwIjoxNjEwNTE3NjUxLCJqdGkiOiJmMTFjNzExN2IzMDFlYTMzYzk0ZmM5NGQzZjRmMjU3N2U5MjIyYWZkIn0.Qv6K2fnoDsK4BqVRRUb5EKsSb6Iu8DPgIdiYLH47qPQXqCaSKrMqPUhbeP1UnaO_P-r97z5bJn-22-K8GzegT2m_EBbVP3z2bKEgbOIsGTTmDaJKzMQzeob9naUv7yAC5mgh4sCloGNhbxHH_l6NzdUgiv03v5oNoL1yQerrdF5pE0plQl6ScFuFKO-lX3OgGsoKXoTOiHmBwZfQ1Z8JLt833erKog0_j7kfPdtHwAGlsXLy_9DNew5fLuy59zg5A8rX-wcyyeLrf4Z7kkbbl4dX1jHx3A_oQUnsbVRARt5x6vMZ23YgTJrhM98RlZuv5IXpqf3m6T9U-fgej26J8Q"
    idpId: "google"
    login_hint: "AJDLj6JUa8yxXrhHdWRHIV0S13cANl8hcyMb6KMyPvHdtUmBomxn_ugOAGOrV6dcWoJZJBTqwfYmpUtEZ0j6rQoebB-fkc2V4A"
    scope: "email profile openid https://www.googleapis.com/auth/userinfo.profile https://www.googleapis.com/auth/userinfo.email"
    session_state:
    extraQueryParams: {authuser: "0"}
    __proto__: Object
    token_type: "Bearer"
  }
Ea: "116392745086041094162"
Nt: Vw {
  EW: "순일"
  Ed: "김순일"
  IU: "김"
  aV: "116392745086041094162"
  fL: "https://lh6.googleusercontent.com/-27Jcxk601ik/AAAAAAAAAAI/AAAAAAAAAAA/AMZuuclzYW_lH8fvdZV0mNLyht6nXwtaQg/s96-c/photo.jpg"
  uu: "sunil.kim@softwareinlife.com"
  }
accessToken: "ya29.a0AfH6SMDox9Gz26d5g_GiTwW-hzxIMyYkpmbIR498Ukz9fSSD4Jd5ugTaPZZ49b73K9rBWm4eqpIyf1262KyqKd1k88kUiRqJ8qTvERzyFf-B9x9ybcSpHuqiIlBQd2bEMuC_FC4IyBtX00qH0muSRcIHq1IvrYrFO7BxsugLzsM"
googleId: "116392745086041094162"
profileObj: {
    email: "sunil.kim@softwareinlife.com"
    familyName: "김"
    givenName: "순일"
    googleId: "116392745086041094162"
    imageUrl: "https://lh6.googleusercontent.com/-27Jcxk601ik/AAAAAAAAAAI/AAAAAAAAAAA/AMZuuclzYW_lH8fvdZV0mNLyht6nXwtaQg/s96-c/photo.jpg"
    name: "김순일"
  }
tokenId: "eyJhbGciOiJSUzI1NiIsImtpZCI6Ijc4M2VjMDMxYzU5ZTExZjI1N2QwZWMxNTcxNGVmNjA3Y2U2YTJhNmYiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJhY2NvdW50cy5nb29nbGUuY29tIiwiYXpwIjoiNzY3NDQzNTgzNTI2LWJiZmpoMnZrOTE5NnJuYWFrY3BndnZqcDR2dW5pdmp0LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwiYXVkIjoiNzY3NDQzNTgzNTI2LWJiZmpoMnZrOTE5NnJuYWFrY3BndnZqcDR2dW5pdmp0LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwic3ViIjoiMTE2MzkyNzQ1MDg2MDQxMDk0MTYyIiwiaGQiOiJzb2Z0d2FyZWlubGlmZS5jb20iLCJlbWFpbCI6InN1bmlsLmtpbUBzb2Z0d2FyZWlubGlmZS5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYXRfaGFzaCI6IlozcnJnTXN2cmFIUXl6MG9XRkRyVXciLCJuYW1lIjoi6rmA7Iic7J28IiwicGljdHVyZSI6Imh0dHBzOi8vbGg2Lmdvb2dsZXVzZXJjb250ZW50LmNvbS8tMjdKY3hrNjAxaWsvQUFBQUFBQUFBQUkvQUFBQUFBQUFBQUEvQU1adXVjbHpZV19sSDhmdmRaVjBtTkx5aHQ2blh3dGFRZy9zOTYtYy9waG90by5qcGciLCJnaXZlbl9uYW1lIjoi7Iic7J28IiwiZmFtaWx5X25hbWUiOiLquYAiLCJsb2NhbGUiOiJrbyIsImlhdCI6MTYxMDUxNDA1MSwiZXhwIjoxNjEwNTE3NjUxLCJqdGkiOiJmMTFjNzExN2IzMDFlYTMzYzk0ZmM5NGQzZjRmMjU3N2U5MjIyYWZkIn0.Qv6K2fnoDsK4BqVRRUb5EKsSb6Iu8DPgIdiYLH47qPQXqCaSKrMqPUhbeP1UnaO_P-r97z5bJn-22-K8GzegT2m_EBbVP3z2bKEgbOIsGTTmDaJKzMQzeob9naUv7yAC5mgh4sCloGNhbxHH_l6NzdUgiv03v5oNoL1yQerrdF5pE0plQl6ScFuFKO-lX3OgGsoKXoTOiHmBwZfQ1Z8JLt833erKog0_j7kfPdtHwAGlsXLy_9DNew5fLuy59zg5A8rX-wcyyeLrf4Z7kkbbl4dX1jHx3A_oQUnsbVRARt5x6vMZ23YgTJrhM98RlZuv5IXpqf3m6T9U-fgej26J8Q"
tokenObj:{
  access_token: "ya29.a0AfH6SMDox9Gz26d5g_GiTwW-hzxIMyYkpmbIR498Ukz9fSSD4Jd5ugTaPZZ49b73K9rBWm4eqpIyf1262KyqKd1k88kUiRqJ8qTvERzyFf-B9x9ybcSpHuqiIlBQd2bEMuC_FC4IyBtX00qH0muSRcIHq1IvrYrFO7BxsugLzsM"
  expires_at: 1610517650513
  expires_in: 3599
  first_issued_at: 1610514051513
  id_token: "eyJhbGciOiJSUzI1NiIsImtpZCI6Ijc4M2VjMDMxYzU5ZTExZjI1N2QwZWMxNTcxNGVmNjA3Y2U2YTJhNmYiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJhY2NvdW50cy5nb29nbGUuY29tIiwiYXpwIjoiNzY3NDQzNTgzNTI2LWJiZmpoMnZrOTE5NnJuYWFrY3BndnZqcDR2dW5pdmp0LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwiYXVkIjoiNzY3NDQzNTgzNTI2LWJiZmpoMnZrOTE5NnJuYWFrY3BndnZqcDR2dW5pdmp0LmFwcHMuZ29vZ2xldXNlcmNvbnRlbnQuY29tIiwic3ViIjoiMTE2MzkyNzQ1MDg2MDQxMDk0MTYyIiwiaGQiOiJzb2Z0d2FyZWlubGlmZS5jb20iLCJlbWFpbCI6InN1bmlsLmtpbUBzb2Z0d2FyZWlubGlmZS5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiYXRfaGFzaCI6IlozcnJnTXN2cmFIUXl6MG9XRkRyVXciLCJuYW1lIjoi6rmA7Iic7J28IiwicGljdHVyZSI6Imh0dHBzOi8vbGg2Lmdvb2dsZXVzZXJjb250ZW50LmNvbS8tMjdKY3hrNjAxaWsvQUFBQUFBQUFBQUkvQUFBQUFBQUFBQUEvQU1adXVjbHpZV19sSDhmdmRaVjBtTkx5aHQ2blh3dGFRZy9zOTYtYy9waG90by5qcGciLCJnaXZlbl9uYW1lIjoi7Iic7J28IiwiZmFtaWx5X25hbWUiOiLquYAiLCJsb2NhbGUiOiJrbyIsImlhdCI6MTYxMDUxNDA1MSwiZXhwIjoxNjEwNTE3NjUxLCJqdGkiOiJmMTFjNzExN2IzMDFlYTMzYzk0ZmM5NGQzZjRmMjU3N2U5MjIyYWZkIn0.Qv6K2fnoDsK4BqVRRUb5EKsSb6Iu8DPgIdiYLH47qPQXqCaSKrMqPUhbeP1UnaO_P-r97z5bJn-22-K8GzegT2m_EBbVP3z2bKEgbOIsGTTmDaJKzMQzeob9naUv7yAC5mgh4sCloGNhbxHH_l6NzdUgiv03v5oNoL1yQerrdF5pE0plQl6ScFuFKO-lX3OgGsoKXoTOiHmBwZfQ1Z8JLt833erKog0_j7kfPdtHwAGlsXLy_9DNew5fLuy59zg5A8rX-wcyyeLrf4Z7kkbbl4dX1jHx3A_oQUnsbVRARt5x6vMZ23YgTJrhM98RlZuv5IXpqf3m6T9U-fgej26J8Q"
  idpId: "google"
  login_hint: "AJDLj6JUa8yxXrhHdWRHIV0S13cANl8hcyMb6KMyPvHdtUmBomxn_ugOAGOrV6dcWoJZJBTqwfYmpUtEZ0j6rQoebB-fkc2V4A"
  scope: "email profile openid https://www.googleapis.com/auth/userinfo.profile https://www.googleapis.com/auth/userinfo.email"
  session_state:
  extraQueryParams: {authuser: "0"}
  __proto__: Object
  token_type: "Bearer"
}
```

Logout

``` jsx
<GoogleLogout
      clientId={clientId}
      buttonText="Logout"
      onLogoutSuccess={logout}
    >
```

