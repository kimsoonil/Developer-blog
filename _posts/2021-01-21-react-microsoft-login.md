---
layout: post
title:  "React Microsoft login"
categories: JavaScript
tags: React Social login
author: kimsoonil
---

* content
{:toc}

  https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps 에서 앱을 등록하고 clientId 추출
  
  내앱 → 인증 → ID 토큰 , 모든 조직 디렉터리의 계정 체크

  내앱 → 매니페스트 “oauth2AllowIdTokenImplicitFlow”  false → true 변경

해당 프로젝트에 ``npm install react-microsoft-login`` or ``yarn add react-microsoft-login`` 추가

### function 


facebook.js

``` js
 const responseMicrosoft = (response) => {
    console.log(response);
  };
  
<MicrosoftLogin clientId={clientId} authCallback={responseFacebook} />

```

response

``` js
{
  accessToken: "eyJ0eXAiOiJKV1QiLCJub25jZSI6Il9VeWxsaWdST1pXUkw3ZDNmSndmV0tpWDFkYk5YeERkZkZOMHFLdUM5eVkiLCJhbGciOiJSUzI1NiIsIng1dCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyIsImtpZCI6Im5PbzNaRHJPRFhFSzFqS1doWHNsSFJfS1hFZyJ9.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTAwMDAtYzAwMC0wMDAwMDAwMDAwMDAiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9kNjMwY2RiMC1mYjUzLTQxZjgtYmEyMi0wMzM5OWNlNWIwNDYvIiwiaWF0IjoxNjEwNTA1NjU2LCJuYmYiOjE2MTA1MDU2NTYsImV4cCI6MTYxMDUwOTU1NiwiYWNjdCI6MCwiYWNyIjoiMSIsImFjcnMiOlsidXJuOnVzZXI6cmVnaXN0ZXJzZWN1cml0eWluZm8iLCJ1cm46bWljcm9zb2Z0OnJlcTEiLCJ1cm46bWljcm9zb2Z0OnJlcTIiLCJ1cm46bWljcm9zb2Z0OnJlcTMiLCJjMSIsImMyIiwiYzMiLCJjNCIsImM1IiwiYzYiLCJjNyIsImM4IiwiYzkiLCJjMTAiLCJjMTEiLCJjMTIiLCJjMTMiLCJjMTQiLCJjMTUiLCJjMTYiLCJjMTciLCJjMTgiLCJjMTkiLCJjMjAiLCJjMjEiLCJjMjIiLCJjMjMiLCJjMjQiLCJjMjUiXSwiYWlvIjoiRTJKZ1lIaFlKK0VpWE5VdSsxQlRNRU5uMFU1QnRRMFJNNDVaWGpqMmY4N1pUYmthak5jQiIsImFtciI6WyJwd2QiXSwiYXBwX2Rpc3BsYXluYW1lIjoicmVhY3QtbWljcm9zb2Z0LWxvZ2luIiwiYXBwaWQiOiJkZGE5ZGM5Ni0wNjNlLTQzMDItYmY0Ny0wZDk4NTE5NDFiOTQiLCJhcHBpZGFjciI6IjAiLCJmYW1pbHlfbmFtZSI6Iuq5gCIsImdpdmVuX25hbWUiOiLsiJzsnbwiLCJpZHR5cCI6InVzZXIiLCJpcGFkZHIiOiIxMjEuMTM0LjE1OC4zNCIsIm5hbWUiOiLquYDsiJzsnbwiLCJvaWQiOiI5NjYzYjA3NS1mNzZmLTRhNGItOTIwMi1lM2ExZjhmYTk4MDYiLCJwbGF0ZiI6IjUiLCJwdWlkIjoiMTAwMzIwMDBGNEQ2MjQyMyIsInJoIjoiMC5BU29Bc00wdzFsUDctRUc2SWdNNW5PV3dScGJjcWQwLUJnSkR2MGNObUZHVUc1UXFBRzQuIiwic2NwIjoib3BlbmlkIHByb2ZpbGUgVXNlci5SZWFkIGVtYWlsIiwic2lnbmluX3N0YXRlIjpbImttc2kiXSwic3ViIjoiTlpiazYtYlBLTUZ2LU9rUnlPajhZS3duMWRKeG5tcUNkcnAxVVlmVWE3OCIsInRlbmFudF9yZWdpb25fc2NvcGUiOiJBUyIsInRpZCI6ImQ2MzBjZGIwLWZiNTMtNDFmOC1iYTIyLTAzMzk5Y2U1YjA0NiIsInVuaXF1ZV9uYW1lIjoic3VuaWwua2ltQHNvZnR3YXJlaW5saWZlLmNvbSIsInVwbiI6InN1bmlsLmtpbUBzb2Z0d2FyZWlubGlmZS5jb20iLCJ1dGkiOiJZT1Rzb0RqSUFVT0dLUC0ybkEwVEFBIiwidmVyIjoiMS4wIiwieG1zX3N0Ijp7InN1YiI6Ii05M1ZreWFoN3RURWx6T1d0dzBJSG9iNjMzeU9FX00yRE1kZTRmcVdCUE0ifSwieG1zX3RjZHQiOjE1NTQ0MzkxNjV9.Lgn3bRVT2wq-ae9h1Ql2uqjtnizElvXP5WwiccsW_J0V93CqxUSoMrSeDpYXRk0NYgZQce7fAsINU_phxjzA3ozEWCo8Gfy5WkW4BRCYkM8wkg0e-V9gYF8KcyZeHp-dWU2VgThiqLvP3p8qzSh5KA1BTjh4Vs7p8OFeIiKNMfsdMDx_7-2f9B2UoinOCuZ_x7TxjV15J_76Df7B56GnRxjDbybPINxYgkP_dC61Bap_bGDViyDbC_MitvhknG-3zu1n7n_s7vL_Y9ZSn3C1BNN2rufdWFbLiPWpjaWOBUMw3ceNuoelLPH3yjg7-kEkVRP8Ac8sadbWuUySbh8eqg"
  account: Account {
    accountIdentifier: "9663b075-f76f-4a4b-9202-e3a1f8fa9806"
    environment: "https://login.microsoftonline.com/d630cdb0-fb53-41f8-ba22-03399ce5b046/v2.0"
    homeAccountIdentifier: "OTY2M2IwNzUtZjc2Zi00YTRiLTkyMDItZTNhMWY4ZmE5ODA2.ZDYzMGNkYjAtZmI1My00MWY4LWJhMjItMDMzOTljZTViMDQ2"
    idToken: {aud: "dda9dc96-063e-4302-bf47-0d9851941b94", iss: "https://login.microsoftonline.com/d630cdb0-fb53-41f8-ba22-03399ce5b046/v2.0", iat: 1610505655, nbf: 1610505655, exp: 1610509555, …}
    idTokenClaims: {aud: "dda9dc96-063e-4302-bf47-0d9851941b94", iss: "https://login.microsoftonline.com/d630cdb0-fb53-41f8-ba22-03399ce5b046/v2.0", iat: 1610505655, nbf: 1610505655, exp: 1610509555, …}
    name: "김순일"
    sid: undefined
    userName: "sunil.kim@softwareinlife.com"
  }
  accountState: "eyJpZCI6Ijk0OWY3MmJlLWI1ZDQtNDIyOS1iODQ1LTIwZGYzYmI1N2Y3YSIsInRzIjoxNjEwNTA1OTU2LCJtZXRob2QiOiJwb3B1cEludGVyYWN0aW9uIn0="
  expiresOn: Wed Jan 13 2021 12:45:55 GMT+0900 (대한민국 표준시) {}
  fromCache: false
  idToken: IdToken {rawIdToken: "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Im5Pbz…SS6LcfeKCK2wXzuJ4tl4qHkUddomxAghDSz7YdbslwbKVwtjw", claims: {…}, issuer: "https://login.microsoftonline.com/d630cdb0-fb53-41f8-ba22-03399ce5b046/v2.0", objectId: "9663b075-f76f-4a4b-9202-e3a1f8fa9806", subject: "-93Vkyah7tTElzOWtw0IHob633yOE_M2DMde4fqWBPM", …}
  idTokenClaims: {aud: "dda9dc96-063e-4302-bf47-0d9851941b94", iss: "https://login.microsoftonline.com/d630cdb0-fb53-41f8-ba22-03399ce5b046/v2.0", iat: 1610505655, nbf: 1610505655, exp: 1610509555, …}
  scopes: (4) ["openid", "profile", "User.Read", "email"]
  tenantId: "d630cdb0-fb53-41f8-ba22-03399ce5b046"
  tokenType: "access_token"
  uniqueId: "9663b075-f76f-4a4b-9202-e3a1f8fa9806"
}
```

Logout

``` js
msalInstance.logout();

```

