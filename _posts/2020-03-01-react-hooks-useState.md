---
layout: post
title:  "React Hooks useState"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}


React Hooks API를 마스터하면 작업에서 더 잘 사용할 수 있고 React를 더 높은 수준으로 마스터 할 수 있습니다. 이 시리즈에서는 초보자와 리뷰어가 사용하기 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

전제는 React Class를 작성하고`setState ()`와 props를 사용하는 방법을 알아야한다는 것입니다.

첫 번째 예부터 시작하겠습니다.




## 간단한 useState-Counter 예제

+1 카운터 클릭

### Class 구성 요소 작성 방법

CounterClass.tsx

``` jsx
import React, { Component } from 'react'

class CounterClass extends Component {

  state = {
    count: 0
  }

  incrementCount = () => {
    this.setState({
      count: this.state.count + 1
    })
  }

  render() {
    const { count } = this.state
    return (
      <div>
        <button onClick={this.incrementCount}>Count {count}</button>
      </div>
    )
  }
}

export default CounterClass

```

App.tsx

``` jsx
import React from 'react'

import './App.css'

import CounterClass from './components/CounterClass'

const App = () => {
  return (
    <div className="App">
      <CounterClass />
    </div>
  )
}

export default App
```

효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1oZQLveL2gK0jSZPhXXahvXXa-306-179.gif)

이러한 카운터를 만드는 것은 3 단계로 나뉩니다.

1. 클래스 구성 요소 만들기
2. 상태 만들기
3. 증분 방법 만들기

다음으로, 함수 컴포넌트와 상태 후크를 사용하여

### State Hook 구현

HookCounter.tsx

``` jsx
import React, { useState } from 'react'

function HookCounter() {
  const [count, setCount] = useState(0)
  return (
    <div>
      <button onClick={() => {
        setCount(count + 1)
      }}>Count {count}</button>
    </div>
  )
}

export default HookCounter
```

App.tsx

``` jsx
import React from 'react'

import './App.css'

// import CounterClass from './components/1CounterClass'
import HookCounter from './components/1HookCounter'

const App = () => {
  return (
    <div className="App">
      <HookCounter />
    </div>
  )
}

export default App

```

효과는 Class 컴포넌트의 효과와 동일합니다.

![](https://gw.alicdn.com/tfs/TB1oZQLveL2gK0jSZPhXXahvXXa-306-179.gif)

useState를 사용하는 방법을 살펴 보겠습니다.

``` js
const [state, setState] = useState(initialState)
```

이 코드 줄은 상태를 업데이트하는 상태와 함수를 반환합니다.

초기 렌더링 중에 반환 된 상태 (상태)는 전달 된 첫 번째 매개 변수 (initialState)와 동일한 값을 갖습니다.

setState 함수는 상태를 업데이트하는 데 사용됩니다. 새 상태 값을 수신하고 구성 요소의 다시 렌더링을 대기열에 넣습니다.

### Hooks 사용 규칙

-Hooks는 최상위 범위에서만 호출 할 수 있습니다.
  -내부 루프, 조건부 판단 및 중첩 방법에서 사용할 수 없습니다.
-Hooks는 React Function에서만 사용할 수 있습니다.
  -다른 일반 기능에서 Hooks를 사용할 수 없습니다.

## useState with Previous State

이 섹션에서는 이전 상태를 사용하는 방법에 대해 설명합니다. 상태 값이 이전 상태 값에 의존하는 경우 이전 상태가 사용됩니다.

카운터 예제를 통해 +1 또는 -1 버튼 클릭 추가

### Counter

HookCounter.tsx

``` jsx
import React, { useState } from 'react'

function HookCounter() {

  const initialCount = 0
  const [count, setCount] = useState(initialCount)

  return (
    <div>
      Count: {count}
      <button onClick={() => {
        setCount(initialCount)
      }}>Reset</button>
      <button onClick={() => {
        setCount(count + 1)
      }}> + 1 </button>
      <button onClick={() => {
        setCount(count - 1)
      }}> - 1 </button>
    </div>
  )
}

export default HookCounter
```

App.tsx

``` jsx
import React from 'react'

import './App.css'

import HookCounter from './components/3HookCounter'

const App = () => {
  return (
    <div className="App">
      <HookCounter />
    </div>
  )
}

export default App
```

효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB13LIYva61gK0jSZFlXXXDKFXa-306-179.gif)

효과에 문제가없는 것 같지만 이렇게 쓰는 것은 안전하지 않습니다! **이것은 카운터를 변경하는 올바른 방법이 아닙니다.** 그 이유를 말씀 드리겠습니다.

위의 예에서는 버튼을 하나 더 추가하고 5 개를 추가 할 때마다


``` diff
import React, { useState } from 'react'

function HookCounter() {

  const initialCount = 0
  const [count, setCount] = useState(initialCount)

+  const increment5 = () => {
+    for (let i = 0; i < 5; i++) {
+      setCount(count + 1)
+    }
+  }

  return (
    <div>
      Count: {count}
      <button onClick={() => {
        setCount(initialCount)
      }}>Reset</button>
      <button onClick={() => {
        setCount(count + 1)
      }}> + 1 </button>
      <button onClick={() => {
        setCount(count - 1)
      }}> - 1 </button>
+      <button onClick={increment5}> + 5 </button>
    </div>
  )
}

export default HookCounter
```

+5를 클릭하면 1 개만 추가됩니다.

![](https://gw.alicdn.com/tfs/TB1AdMTvoz1gK0jSZLeXXb9kVXa-306-179.gif)

이는 setCount 메서드가 비동기식으로 즉시 반응하고 업데이트 할 수 없기 때문이며, 한 번에 여러 번 전송되는 카운트는 여전히 이전 값이며 업데이트되지 않았습니다.

다음과 같이 코드를 수정하십시오.

``` diff
import React, { useState } from 'react'

function HookCounter() {

  const initialCount = 0
  const [count, setCount] = useState(initialCount)

  const increment5 = () => {
    for (let i = 0; i < 5; i++) {
+      setCount(prevCount => prevCount + 1)
    }
  }

  return (
    <div>
      Count: {count}
      <button onClick={() => {
        setCount(initialCount)
      }}>Reset</button>
      <button onClick={() => {
        setCount(count + 1)
      }}> + 1 </button>
      <button onClick={() => {
        setCount(count - 1)
      }}> - 1 </button>
      <button onClick={increment5}> + 5 </button>
    </div>
  )
}

export default HookCounter
```

![](https://gw.alicdn.com/tfs/TB1ZG3WvkL0gK0jSZFAXXcA9pXa-306-179.gif)

이전 상태를 사용하려면 함수를 통해 값을 전달하고 변경 후 새로운 값으로 복귀해야합니다.`+ 1``-1`의 함수도 수정됩니다. 개선 된 코드는 다음과 같습니다.

``` jsx
import React, { useState } from 'react'

function HookCounter() {

  const initialCount = 0
  const [count, setCount] = useState(initialCount)

  const increment5 = () => {
    for (let i = 0; i < 5; i++) {
      setCount(prevCount => prevCount + 1)
    }
  }

  return (
    <div>
      Count: {count}
      <button onClick={() => {
        setCount(initialCount)
      }}>Reset</button>
      <button onClick={() => {
        setCount(prevCount => prevCount + 1)
      }}> + 1 </button>
      <button onClick={() => {
        setCount(prevCount => prevCount - 1)
      }}> - 1 </button>
      <button onClick={increment5}> + 5 </button>
    </div>
  )
}

export default HookCounter
```

### 정리

previousState를 사용하는 경우 setter 함수를 사용하고 setState 메서드에 매개 변수를 전달합니다. 정확한 이전 상태를 확인하기 위해.

다시 렌더링 할 때 useState가 반환하는 첫 번째 값은 항상 업데이트 후 최신 상태가됩니다.

## useState with Object

useState의 상태가 객체 인 경우 해당 setState를 호출하면 몇 가지주의 사항이 있습니다. UseState는 객체를 자동으로 병합 및 업데이트하지 않습니다. 확장 연산자와 결합 된 함수 setState를 사용하여 객체 병합 및 업데이트 효과를 얻을 수 있습니다.

### 오류 예 firstName & lastName

HookCounter.tsx

``` jsx
import React, { useState } from 'react'

function HookCounter() {

  const [name, setName] = useState({
    firstName: '',
    lastName: ''
  })

  return (
    <form>
      <input
        type="text"
        value={name.firstName}
        onChange={e => {
          setName({
            firstName: e.target.value
          })
        }}
      />
      <input
        type="text"
        value={name.lastName}
        onChange={e => {
          setName({
            lastName: e.target.value
          })
        }}
      />
      <h2>Your first name is {name.firstName}</h2>
      <h2>Your last name is {name.lastName}</h2>
    </form>
  )
}

export default HookCounter
```

입력 태그의 onChange에서 setName을 설정할 때마다 객체의 한 속성 만 조작됩니다. firstName에 값을 할당 할 때만 lastName 속성이 사라 지므로 이는 잘못된 작성 방법입니다.

tsx를 사용하여 구성 요소를 작성했기 때문에 컴파일러에서 직접 오류를보고했습니다.

![](https://gw.alicdn.com/tfs/TB1q8E0voY1gK0jSZFCXXcwqXXa-1098-822.jpg)

console 창을 확인해보면

![](https://gw.alicdn.com/tfs/TB1FIMVvhD1gK0jSZFsXXbldVXa-910-832.jpg)

### 수동 병합 개체 수정

이 객체의 문제를 해결하기 위해 확산 연산자를 사용하려면

``` jsx
import React, { useState } from 'react'

function HookCounter() {

  const [name, setName] = useState({
    firstName: '',
    lastName: ''
  })

  return (
    <form>
      <input
        type="text"
        value={name.firstName}
        onChange={e => {
          setName({
            ...name,
            firstName: e.target.value
          })
        }}
      />
      <input
        type="text"
        value={name.lastName}
        onChange={e => {
          setName({
            ...name,
            lastName: e.target.value
          })
        }}
      />
      <h2>Your first name is {name.firstName}</h2>
      <h2>Your last name is {name.lastName}</h2>
      <h2>{JSON.stringify(name)}</h2>
    </form>
  )
}

export default HookCounter
```

![](https://gw.alicdn.com/tfs/TB1mfIUvXY7gK0jSZKzXXaikpXa-373-215.gif)

### 정리

state hooks 에서 객체를 조작 할 때 객체의 속성은 자동으로 병합되지 않으므로 수동으로 병합해야합니다. 스프레드 연산자를 사용할 수 있습니다.


## useState with Array

### 목록

버튼을 클릭하여 1-10의 임의의 숫자를 목록에 추가하십시오.

UseStateWithArray.tsx

``` jsx
import React, { useState } from 'react'

interface ItemType {
  id: number
  value: number
}

function UseStateWithArray() {
  const [items, setItems] = useState<ItemType[]>([])

  const addItem = () => {
    setItems([
      ...items,
      {
        id: items.length,
        value: Math.ceil(Math.random() * 10)
      }
    ])
  }

  return (
    <div>
      <button onClick={addItem}>add a number</button>
      <ul>
        {
          items.length > 0 && items.map((item: ItemType) => (
            <li key={item.id}>{item.value}</li>
          ))
        }
      </ul>
    </div>
  )
}

export default UseStateWithArray
```

정리

![](https://gw.alicdn.com/tfs/TB1vcE7vXY7gK0jSZKzXXaikpXa-418-215.gif)

hooks에서 typeScript의 사용법에 유의하십시오. 다음 기사를 참조하십시오.

- [TypeScript 中使用React Hook](https://juejin.im/post/5ce0134b5188256a220235eb)

## useState 

이것이 useState의 사용을위한 것입니다. 여기에 간략한 요약이 있습니다.

-기능 구성 요소에서 상태를 사용할 수 있습니다.
-클래스 구성 요소에서 state는 객체이지만 useState에서는 state가 객체가 아닐 수 있으며 모든 유형이 될 수 있습니다.
-useState는 두 요소의 배열을 반환합니다.
  -첫 번째는 상태의 현재 값입니다.
  -두 번째는 상태의 setter 메소드로 호출시 다시 렌더링을 트리거합니다.
    -현재 상태가 이전 상태에 의존하는 경우 상태의 setter 메소드에 함수를 전달하고 입력 매개 변수가 이전 상태이며 새로운 상태를 반환 할 수 있습니다.
-객체 및 배열의 ​​경우 상태의 이전 변수가 자동으로 완료되지 않으므로 확장 연산자를 사용하여 수동으로 추가해야합니다.

useState에 대한 학습은 여기에 있으며 다음 기사에서는 useEffect를 함께 사용하는 방법을 학습합니다.



