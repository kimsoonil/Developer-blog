---
layout: post
title:  "React Facebook login"
categories: JavaScript
tags: React Facebook login
author: KSI
---

* content
{:toc}

https://github.com/keppelen/react-facebook-login 해당 라이브러리를 적용하였습니다.

https://developers.facebook.com/apps/에서 앱을 만들고 앱ID 추출



![facebook_clientId](https://kimsoonil.github.io/img/facebook_clientId.png)

해당 프로젝트에 ``npm install react-facebook-login`` or ``yarn add react-facebook-login`` 추가

### function 

```js
  import React from 'react';
  import FacebookLogin from 'react-facebook-login';

  class MyComponent extends React.Component {
    responseFacebook(response) {
      console.log(response);
    }

    render() {
      return (
        <FacebookLogin
          appId="1088597931155576"
          autoLoad={true}
          fields="name,email,picture"
          scope="public_profile,user_friends,user_actions.books"
          callback={this.responseFacebook}
        />
      )
    }
  }

  export default MyComponent;
```
git에서 나와있는 코드는 위와 같다 이 코드에서 필요한 것만 내 코드에 적용하였다.

facebook.js

``` js
 const responseFacebook = response => {
        console.log(response);
}
  
  return(
  <FacebookLogin
        appId={appId}
        autoLoad={false} 
        fields="name,email,picture"
        
        callback={responseFacebook} 
        render={renderProps => (
            <button className={classes.facebookBtn}
               onClick={renderProps.onClick}>
               <Icon icon={facebookIcon}
               color={"#fff"}
               width="26" height="26" 
               />
               
            </button>
          )}
        />

```

response

``` js
{
  accessToken: "EAAC8Lo5zQewBAN4W710ZAy6NDQWWw2VI2NZCWBgNg3wl3J6sjZAaaVcaKtsEzpYmenZCtj3aZAFDUSyodqZBSDkqRFBfUsfRHZBpYDig0rftTJQkD5akZAXR6jADFkb0TdeaLh76SB1UU04pKZAepRkTMUfcFLohZARgx59jpRq479cZBm6GIxAT7flkqwmLZCYRmngXZA3MJBk0aaQZDZD"
  data_access_expiration_time: 1618276974
  email: "slskdkfdl@naver.com"
  expiresIn: 5826
  graphDomain: "facebook"
  id: "3820135101398199"
  name: "김순일"
  picture: 
    data:{
      height: 50
      is_silhouette: false
      url: "https://platform-lookaside.fbsbx.com/platform/profilepic/?asid=3820135101398199&height=50&width=50&ext=1613094362&hash=AeQJYMpEBMsBwNwCzZQ"
      width: 50
     }
  signedRequest: "Urywddab952I8d5IHY0WVFiWccYkTOnucSo98x6BVuk.eyJ1c2VyX2lkIjoiMzgyMDEzNTEwMTM5ODE5OSIsImNvZGUiOiJBUUJFamFjQTJxc3hrRmt5c3ZaWmxEN1ZxUGxYS01meHkzb0s1X1RiTmFZblF1SER1U2RWTU45Zk9LU3hKY19SMGJvWWN0YkpSc2FJZmFFVndlT1pLeXBndGkxU1QtNFdNVnpiY3d2NXVNQmFQOHVaNi1YdFVPNVJHUVpyOXZ6MEQtRnNjSV9WazdjR21STEpRVTVkU0pzWmhOSzdibFE5TkwwTGMtNWtOajg3VlJLZDhiaWJlbWhEUWRKOHZxa3d3cTZseUYyaUxHQ2x0MUZzYktsZmxOcWEzR2N2SnU5MWktN2VWZ1hLOEczelJjaG9yX2FkMm51UHJ4dG9oUUxyZS01N216NU0yQ3lVckwwbEZLVFB2c04xck9raS15aXFiazV5SUFLbUJDYzF0dC1ibUZoelZhZC1RRWdab09LRzEtT3ExM1ItX0VYMnZYMjU0SGVZb2RZeSIsImFsZ29yaXRobSI6IkhNQUMtU0hBMjU2IiwiaXNzdWVkX2F0IjoxNjEwNTAwOTc0fQ"
  userID: "3820135101398199"
}
```

Logout

``` js
window.FB.logout();
```

