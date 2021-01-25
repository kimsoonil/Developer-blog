---
layout: post
title:  "React Hooks useContext"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}

React Hooks api를 숙달하면 직장에서 사용하는 데 더 도움이 될 것이며 React에 대한 숙달은 당신을 더 높은 수준으로 이끌 것입니다. 이 시리즈는 초보자와 리뷰어에게 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

오늘 우리는 Context 객체와 useContext의 사용에 대해 이야기합니다.




## Context Api 란?

구성 요소 트리 구조가 다음과 같고 이제 루트 노드에서 리프 노드 ADF로 userName 속성을 전달하여 props를 통해 전달하려는 경우 이러한 구성 요소가 userName 속성을 사용하지 않더라도 불가피하게 BCE를 통과하게됩니다. .
![](https://gw.alicdn.com/tfs/TB1Nl38AAT2gK0jSZFkXXcIQFXa-1432-870.png)

이러한 중첩 된 트리 구조에 5 개 또는 10 개의 레이어가 있으면 치명적인 개발 및 유지 관리 경험이 될 것입니다. 이 문제는 중간 노드를 거치지 않고 원하는 곳에 직접 도달 할 수 있다면 피할 수 있으며, 이때 Context api는이 문제를 해결하는 것입니다.

컨텍스트 API는 모든 계층을 거치지 않고 구성 요소 트리의 데이터를 전달하는 API입니다. Context Hook의 사용을 살펴 보겠습니다.

## 컨텍스트 사용

예를 들어 루트 노드에서 F 노드로 변수 사용자 이름을 전달하는 가장 오른쪽 분기 인 CEF에 중점을 둡니다.

먼저 다음과 같이 App, ComponentC, ComponentE, ComponentF를 만듭니다.

App.js

``` jsx
import React from 'react'

import './App.css'

import ComponentC from './components/16ComponentC'

const App = () => {
  return (
    <div className="App">
      <ComponentC />
    </div>
  )
}

export default App
```

ComponentC.js

```js
import React from 'react'

import ComponentE from './16ComponentE'

function ComponentC() {
  return (
    <div>
      <ComponentE />
    </div>
  )
}

export default ComponentC

```

ComponentE.js

```js
import React from 'react'

import ComponentF from './16ComponentF'

function ComponentE() {
  return (
    <div>
      <ComponentF />
    </div>
  )
}

export default ComponentE

```

ComponentF.js

```js
import React from 'react'

function ComponentF() {
  return (
    <div>
      ComponentF
    </div>
  )
}

export default ComponentF
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1_nrMFkL0gK0jSZFAXXcA9pXa-438-193.png)

다음으로 Context를 사용하여 App에서 ComponentF로 사용자 이름을 전달하는 방법을 살펴 보겠습니다. 다음 3 단계로 나뉩니다.

### 컨텍스트 만들기

루트 노드 App.js 中使用 `createContext()` 를 사용하여 컨텍스트 만들기

``` js
const UserContext = React.createContext('')
```

> Context 개체를 만듭니다. React가이 Context 객체를 구독하는 컴포넌트를 렌더링 할 때,이 컴포넌트는 컴포넌트 트리에서 자신에게 가장 가까운 일치하는 Provider로부터 현재 컨텍스트 값을 읽습니다.
>
> defaultValue 매개 변수는 구성 요소가있는 트리에 일치하는 공급자가없는 경우에만 적용됩니다. 이렇게하면 Provider를 사용하여 구성 요소를 래핑하지 않고도 구성 요소를 테스트 할 수 있습니다. 참고 : undefined가 Provider 값에 전달되면 소비자 구성 요소의 defaultValue가 적용되지 않습니다.

### Provider 제공

Provider를 사용하여 루트 노드에서 자식 노드를 래핑하고 자식 노드에 컨텍스트를 제공합니다.

``` js
<UserContext.Provider value={'chuanshi'}>
  <ComponentC />
</UserContext.Provider>
```

> 각 Context 객체는 Provider React 구성 요소를 반환하므로 소비 구성 요소가 컨텍스트 변경 사항을 구독 할 수 있습니다.
> value소비자 어셈블리에 전달 된 속성을 받는 공급자 입니다. 공급자는 여러 소비자 구성 요소에 해당 할 수 있습니다. 여러 공급자를 중첩 할 수도 있으며 내부 공급자가 외부 데이터를 덮습니다.
> Provider의 value값이 변경되면 모든 구성 요소의 내부 소비가 다시 렌더링됩니다. 공급자 및 내부 구성 요소에는 소비자 shouldComponentUpdate기능이 적용되지 않으므로 상위 구성 요소 의 소비자 구성 요소가 업데이트되면 업데이트 할 수도 있습니다.
> 새 값과 이전 값 감지는 Object.is와 동일한 알고리즘을 사용하여 변경을 결정하는 데 사용됩니다.


하위 노드에 도입 될 수 있도록 이전에 정의 된 컨텍스트를 내보내는 것을 잊지 마십시오.

``` js
export const UserContext = React.createContext('')
```

현재 App.js의 전체 코드는 다음과 같습니다.

``` jsx
import React from 'react'

import './App.css'

import ComponentC from './components/16ComponentC'

export const UserContext = React.createContext('')

const App = () => {
  return (
    <div className="App">
      <UserContext.Provider value={'chuanshi'}>
        <ComponentC />
      </UserContext.Provider>
    </div>
  )
}

export default App
```

### 사용 된 노드에서 Context 사용

import context 对象

``` js
import { UserContext } from '../App'
```

소비를 위해 Consumer 사용

``` js
<UserContext.Consumer>
  {
    (user) => (
      <div>
        User context value {user}
      </div>
    )
  }
</UserContext.Consumer>
```

> 여기서 React 컴포넌트는 컨텍스트 변경을 구독 할 수도 있습니다. 이를 통해 기능 구성 요소의 컨텍스트를 구독 할 수 있습니다.

> 이를 위해서는 어린 시절의 기능 연습이 필요합니다. 이 함수는 현재 컨텍스트 값을 받고 React 노드를 반환합니다. 함수에 전달 된 값은 위쪽 구성 요소 트리에서 컨텍스트에 가장 가까운 공급자가 제공 한 값과 동일합니다. 해당 Provider가없는 경우 value 매개 변수는 createContext ()에 전달 된 defaultValue와 동일합니다.

전체 ComponentF.js 코드는 다음과 같습니다.

``` jsx
import React from 'react'

import { UserContext } from '../App'

function ComponentF() {
  return (
    <div>
      <UserContext.Consumer>
        {
          (user) => (
            <div>
              User context value {user}
            </div>
          )
        }
      </UserContext.Consumer>
    </div>
  )
}

export default ComponentF
```

효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1CSB6FpT7gK0jSZFpXXaTkpXa-448-210.png)

현재 Context가 1 개만 있으면 상황은 괜찮습니다. 여러 Context가있는 상황을 살펴 보겠습니다.

### 여러 Context 싱허ㅣㅇ

App.js에 다른 컨텍스트를 추가합니다.

``` jsx
import React from 'react'

import './App.css'

import ComponentC from './components/16ComponentC'

export const UserContext = React.createContext('')
export const ChannelContext = React.createContext('')

const App = () => {
  return (
    <div className="App">
      <UserContext.Provider value={'chuanshi'}>
        <ChannelContext.Provider value={'code volution'}>
          <ComponentC />
        </ChannelContext.Provider>
      </UserContext.Provider>
    </div>
  )
}

export default App
```

다음으로 component F에서

``` jsx
import React from 'react'

import { UserContext, ChannelContext } from '../App'

function ComponentF() {
  return (
    <div>
      <UserContext.Consumer>
        {
          (user) => (
            <ChannelContext.Consumer>
              {
                (channel) => (
                  <div>
                    User context value {user}, channel value {channel}
                  </div>
                )
              }
            </ChannelContext.Consumer>

          )
        }
      </UserContext.Consumer>
    </div>
  )
}

export default ComponentF
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1XC47Fvb2gK0jSZK9XXaEgFXa-453-183.png)

코드는 문제없이 실행되지만 미학과 가독성은 그리 좋지 않습니다. 여러 Context를 사용하면 Context hook을 사용하여 여러 Context를 소비하는 코드의 우아한 문제를 해결하는 더 좋은 방법이 있습니다.

## useContext

예를 들어, 위 데모의 컴포넌트 E useContext는 루트 노드를 사용하여 생성 된 Context 에 의한 것 입니다. 다음 단계로 나뉩니다.

1. 반응 객체 useContext에서이 후크 API 가져 오기
2. 루트 노드에 의해 생성 된 컨텍스트 개체를 가져옵니다 (여러 개를 가져올 수 있음).
3. 실행 useContext()방법, 들어오는 컨텍스트

ComponentE 전체 코드 :

``` jsx
import React, { useContext } from 'react'

import ComponentF from './16ComponentF'
import {UserContext, ChannelContext} from '../App'

function ComponentE() {
  const user = useContext(UserContext)
  const channel = useContext(ChannelContext)
  return (
    <div>
      <ComponentF />
      --- <br/>
      {user} - {channel}
    </div>
  )
}

export default ComponentE
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1NnVuFUT1gK0jSZFhXXaAtVXa-453-224.png)

코드의 핵심 라인은 다음과 같습니다.

``` js
const value = useContext(MyContext)
```

> useContext 메서드는 컨텍스트 객체 ( React.createContext반환 값)를 받고 컨텍스트의 현재 값을 반환합니다. 가까운 전류 성분으로부터 상기 상부 어셈블리의 현재 컨텍스트 값 소품 결정.<MyContext.Provider>value
>
> <MyContext.Provider>후크에 대한 상위 어셈블리의 최근 업데이트가 다시 렌더링을 트리거하고 값 의 MyContext컨텍스트 공급자에 전달 된 최신을 사용합니다 value. 사용하더라도 조상 React.memo또는 shouldComponentUpdate자체가 구성 요소의 사용 useContext시간을 렌더링 다시.
>
> 그것은로 이해 될 수 useContext(MyContext)클래스 구성 요소들에 대응 static contextType = MyContext하거나 <MyContext.Consumer>.
> 
> useContext(MyContext)컨텍스트의 값을 읽고 컨텍스트의 변경 사항을 구독 할 수 있습니다. <MyContext.Provider>기본 구성 요소에 대한 컨텍스트를 제공 하려면 상위 구성 요소 트리를 사용해야합니다 .

지금까지 useContext 후크 api의 사용을 마스터했으며 useContext가 여러 컨텍스트에서 사용하는 코드의 복잡성을 크게 줄일 수 있음을 알 수 있습니다.
