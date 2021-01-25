---
layout: post
title:  "React Hooks useEffect"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}

React Hooks API를 마스터하면 작업에서 더 잘 사용할 수 있고 React를 더 높은 수준으로 마스터 할 수 있습니다. 이 시리즈에서는 초보자와 리뷰어가 사용하기 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

오늘 우리는 useEffect를 사용하는 방법에 대해 이야기합니다.





## useEffect를 사용하는 이유

### 라이프 사이클에서 논리 작성 문제

React의 이전 라이프 사이클은 부작용이있을 수 있습니다. 예를 들어 페이지 제목에 클릭 횟수를 표시하려는 경우 코드는 다음과 같습니다.

``` js
componentDidMount() {
  document.title = `${this.state.count} times`
}
componentDidUpdate() {
  document.title = `${this.state.count} times`
}
```

componentDidMount와 componentDidUpdate에 동일한 코드가 작성되어 컴포넌트의 라이프 사이클에서 한 번 마운트 할 수 없어 코드 중복 문제가 발생합니다.


페이지에 카운트 다운 타이머가 포함되어 있으며 페이지가 삭제되면 카운트 다운 타이머를 지워야합니다.

``` js
componentDidMount() {
  this.interval = setInterval(this.tick, 1000)
}
componentWillUnmount() {
  clearInterval(this.interval)
}
```

이 구성 요소가 더 복잡하고 위의 두 논리를 포함하는 경우 코드는 다음과 같습니다.

``` js
componentDidMount() {
  document.title = `${this.state.count} times`
  this.interval = setInterval(this.tick, 1000)
}
componentDidUpdate() {
  document.title = `${this.state.count} times`
}
componentWillUnmount() {
  clearInterval(this.interval)
}
```


우리는 2 개의 문제를 본다

1. 코드가 중복되었습니다. 제목을 설정하는 코드가 1 회 반복됩니다.
2. 코드가 흩어져 있습니다. 로직은 구성 요소 수명주기의 여러 위치에 흩어져있는 것 같습니다.

따라서 우리는 더 나은 솔루션이 필요합니다

### useEffect로 해결 된 문제

- EffectHook은 기능성 구성 요소의 부작용에 사용되며 일부 관련 작업을 수행하며 위의 문제를 해결합니다.
- componentDidMount, componentDidUpdate, componentWillUnmount의 대체물로 간주 될 수 있습니다.

다음으로 useEffect를 사용하는 방법을 알아 봅니다.

## 렌더링 후 useEffect

예를 들어 버튼을 클릭하여 페이지 제목을 수정하는 예를 통해

### 클래스 컴포넌트 작성 예제

ClassCounter.js

``` jsx
import React, { Component } from 'react'

class ClassCounter extends Component {

  state = {
    count: 0
  }

  componentDidMount() {
    document.title = `${this.state.count} times`
  }
  componentDidUpdate() {
    document.title = `${this.state.count} times`
  }

  render() {
    return (
      <div>
        <button onClick={() => {
          this.setState({
            count: this.state.count + 1
          })
        }}>
          Clicked {this.state.count} times
        </button>

      </div>
    )
  }
}

export default ClassCounter
```

App.js

``` jsx
import React from 'react'

import './App.css'

import ClassCounter from './components/7ClassCounter'

const App = () => {
  return (
    <div className="App">
      <ClassCounter />
    </div>
  )
}

export default App

```

효과는 다음과 같습니다.

! [] (https://gw.alicdn.com/tfs/TB1yhhavy_1gK0jSZFqXXcpaXXa-418-215.gif)

### useEffect를 사용하여 위의 예를 다시 작성하십시오.

다음으로 함수를 사용할 때 컴포넌트는 위의 예제를 구현합니다.

HookCounter.js

``` jsx
import React, { useState, useEffect } from 'react'

function HookCounter() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    document.title = `${count} times`
  })

  return (
    <div>
      <button onClick={() => {
        setCount(prevCount => prevCount + 1)
      }} >Clicked {count} times</button>
    </div>
  )
}

export default HookCounter
```

효과는 Class 컴포넌트와 동일합니다.

![](https://gw.alicdn.com/tfs/TB1yhhavy_1gK0jSZFqXXcpaXXa-418-215.gif)

useEffect의 첫 번째 입력 매개 변수가 각 렌더링 후에 호출되는 익명 함수임을 알 수 있습니다. 첫 번째 렌더링 및 후속 업데이트 렌더링이 모두 호출됩니다.

또한 useEffect는 기능적 구성 요소로 작성되므로 이와 같은 코드를 작성하지 않고도 props 및 state의 값을 직접 가져올 수 있습니다.

## 조건부로 useEffect 실행

이전 섹션에서 useEffect가 렌더링 할 때마다 내부 함수를 실행한다는 것을 배웠습니다. 이로 인해 성능 문제가 발생할 수 있습니다. 다음으로 useEffect에서 익명 함수를 조건부로 실행하는 방법에 대해 설명하겠습니다.

이전 섹션의 예에서 입력 이름의 기능을 확장하고 판단에 의해 카운트 변경으로 가져온 로직 만 실행합니다.

### 클래스 컴포넌트 작성 예제

``` jsx
import React, { Component } from 'react'

interface stateType {
  count: number
  name: string
}

class ClassCounter extends Component {

  state = {
    count: 0,
    name: '',
  }

  componentDidMount() {
    document.title = `${this.state.count} times`
  }

  componentDidUpdate(prevProps: any, prevState: stateType) {
    if (prevState.count !== this.state.count) {
      console.log('Update title')
      document.title = `${this.state.count} times`
    }
  }

  render() {
    return (
      <div>
        <input
          type="text"
          value={this.state.name}
          onChange={(e) => {
            this.setState({
              name: e.target.value
            })
          }}
        />
        <button onClick={() => {
          this.setState({
            count: this.state.count + 1
          })
        }}>
          Clicked {this.state.count} times
        </button>
      </div>
    )
  }
}

export default ClassCounter
```

![](https://gw.alicdn.com/tfs/TB1aX7.voz1gK0jSZLeXXb9kVXa-610-312.gif)

더 나은 성능을 위해 prevState는 코드에서 판단됩니다.

### useEffect 작성 방법

``` jsx
import React, { useState, useEffect } from 'react'

function HookCounter() {
  const [count, setCount] = useState(0)
  const [name, setName] = useState('')

  useEffect(() => {
    console.log('useEffect - update title')
    document.title = `You clicked ${count} times`
  }, [count])

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => {
          setName(e.target.value)
        }}
      />
      <button onClick={() => {
        setCount(prevCount => prevCount + 1)
      }} >Clicked {count} times</button>
    </div>
  )
}

export default HookCounter
```

![](https://gw.alicdn.com/tfs/TB1efJbvAY2gK0jSZFgXXc5OFXa-610-312.gif)

useEffect의 두 번째 매개 변수 `[count]`는 배열이고 요소는 관찰 할 상태 또는 소품이며 지정된 변수가 변경 될 때만 useEffect의 첫 번째 매개 변수가 익명으로 트리거됩니다. 함수의 실행. 이는 성능 보장에 도움이됩니다.

## useEffect 한 번만 실행

이 섹션에서는 마우스 좌표를 기록하는 예제를 사용하여 useEffect를 한 번만 실행하는 방법을 연구합니다.

### 마우스 위치 기록 예 클래스 작성

``` jsx
import React, { Component } from 'react'

class RunEffectsOnlyOnce extends Component {
  state = {
    x: 0,
    y: 0
  }

  logMousePos = (e: MouseEvent) => {
    this.setState({
      x: e.clientX,
      y: e.clientY
    })
  }

  componentDidMount() {
    document.addEventListener('mousemove', this.logMousePos)
  }

  render() {
    return (
      <div>
        Y - {this.state.y}, X - {this.state.x}
      </div>
    )
  }
}

export default RunEffectsOnlyOnce
```

![](https://gw.alicdn.com/tfs/TB1ydpavET1gK0jSZFrXXcNCXXa-295-225.gif)

여기서 componentDidMount에서는 이벤트 바인딩 만 수행되고 이벤트 바인딩은 한 번만 수행됩니다.

### useEffect에서 마우스 좌표 기록

위의 효과는 기능적 구성 요소로 변환됩니다.

``` jsx
import React, { useState, useEffect } from 'react'

function RunEffectsOnlyOnce() {

  const [x, setX] = useState(0)
  const [y, setY] = useState(0)

  const logMousePos = (e: MouseEvent) => {
    setX(e.clientX)
    setY(e.clientY)
  }

  useEffect(() => {
    console.log('addEventListener')
    document.addEventListener('mousemove', logMousePos)
  }, [])

  return (
    <div>
      Y - {y}, X - {x}
    </div>
  )
}

export default RunEffectsOnlyOnce
```

useEffect 메서드의 두 번째 매개 변수는 빈 배열로 전달되므로 여러 호출 문제를 효과적으로 방지 할 수 있습니다.

> 한 번만 실행되는 이펙트 (컴포넌트가 마운트 및 마운트 해제 될 때만 실행 됨)를 실행하려면 빈 배열 ([])을 두 번째 매개 변수로 전달할 수 있습니다. 
>
> 이것은 React에 효과가 props 또는 state의 값에 의존하지 않으므로 반복적으로 실행할 필요가 없음을 알려줍니다. 이것은 특별한 경우가 아니며 종속 배열로 작업하는 방식을 따릅니다.
>
> 빈 배열 ([])을 전달하면 효과 내부의 소품과 상태는 항상 초기 값을 갖습니다. 
>
> 두 번째 매개 변수로 []를 전달하는 것이 더 익숙한`componentDidMount` 및`componentWillUnmount` 사고 모드에 더 가깝지만 효과를 너무 자주 호출하지 않도록하는 더 좋은 방법이 있습니다. 
> 또한 브라우저가 화면 렌더링을 마친 후 React는`useEffect` 호출을 지연하므로 추가 작업이 매우 편리합니다.

## 지울 효과

이 섹션에서는 willUnmount의 수명주기를 구현하는 방법과 구성 요소가 파괴 될 때 효과 논리를 지우는 방법을 연구합니다.

이전 데모에 논리를 추가하고 버튼을 클릭하여 마우스의 좌표 구성 요소를 표시하거나 숨 깁니다.

### 구성 요소 표시 및 제거

3 개의 파일이 있으며 구조는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1R_pPFHY1gK0jSZTEXXXDQVXa-309-277.png)


App.js
``` jsx
import React from 'react'

import './App.css'

import MouseContainer from './components/10MouseContainer'

const App = () => {
  return (
    <div className="App">
      <MouseContainer />
    </div>
  )
}

export default App

```

MouseContainer.js

``` jsx
import React, { useState } from 'react'

import RunEffectsOnlyOnce from './9RunEffectsOnlyOnce'

function MouseContainer() {
  const [display, setDisplay] = useState(true)
  return (
    <div>
      <button onClick={() => setDisplay(!display)}>Toggle display</button>
      {display && <RunEffectsOnlyOnce />}
    </div>
  )
}

export default MouseContainer
```

MousePos

``` jsx
import React, { useState, useEffect } from 'react'

function RunEffectsOnlyOnce() {

  const [x, setX] = useState(0)
  const [y, setY] = useState(0)

  const logMousePos = (e: MouseEvent) => {
    setX(e.clientX)
    setY(e.clientY)
  }

  useEffect(() => {
    console.log('addEventListener')
    document.addEventListener('mousemove', logMousePos)
  }, [])

  return (
    <div>
      Y - {y}, X - {x}
    </div>
  )
}

export default RunEffectsOnlyOnce
```

![](https://gw.alicdn.com/tfs/TB1nYXavEH1gK0jSZSyXXXtlpXa-579-284.gif)

실행 후 위치 구성 요소가 숨겨져 있으며 오류 경고가 발생했습니다. 이는 하위 구성 요소를 잘못 제거했기 때문입니다. mousemove 이벤트는 여전히 모니터링 및 실행 중입니다. 그리고 메모리 누수가 발생할 수 있습니다.

### componentWillUnmount

따라서 구성 요소를 제거 할 때 모든 모니터와 구독이 제거되었는지 확인하십시오. Class 컴포넌트에있는 경우 다음과 같이 코딩 할 수 있습니다.

``` js
componentWillUnmount() {
  document.removeEventListener('mousemove', this.logMousePos)
}
```

그러나 useEffect를 작성하는 방법은 무엇입니까? 아래를 봐주세요

``` jsx
  useEffect(() => {
    console.log('addEventListener')
    document.addEventListener('mousemove', logMousePos)
    return () => {
      document.removeEventListener('mousemove', logMousePos)
    }
  }, [])
```

useEffect의 첫 번째 매개 변수에 익명 반환 함수를 추가합니다.이 익명 함수는 구성 요소가 제거 될 때 실행되므로 여기서 리스너를 제거합니다.

![](https://gw.alicdn.com/tfs/TB1eAcBvX67gK0jSZPfXXahhFXa-579-284.gif)

구성 요소를 제거 할 때 함수를 지우는 코드가 필요한 경우 useEffect의 첫 번째 매개 변수의 반환 익명 함수에 작성하십시오.

## useEffect의 종속성 오류로 인한 버그

useEffect 종속성 (두 번째 매개 변수) 오류로 인한 문제점입니다.

초당 +1 카운터를 예로 들어 보겠습니다.

### 클래스 구성 요소 예

Counter.js

``` jsx
import React, { Component } from 'react'

class Counter extends Component {

  state = {
    count: 0
  }

  timer: number | undefined

  tick = () => {
    this.setState({
      count: this.state.count + 1
    })
  }

  componentDidMount() {
    this.timer = window.setInterval(this.tick, 1000)
  }

  componentWillUnmount() {
    clearInterval(this.timer)
  }


  render() {
    return (
      <div>
        <span>{this.state.count}</span>
      </div>
    )
  }
}

export default Counter

```

App.js

``` jsx
import React from 'react'

import './App.css'

import IntervalCounter from './components/11Counter'

const App = () => {
  return (
    <div className="App">
      <IntervalCounter />
    </div>
  )
}

export default App

```

구현에 문제가 없으며 효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1CiNxx9f2gK0jSZFPXXXsopXa-487-270.gif)

### hooks 

IntervalCounterHooks.js

``` jsx
import React, { useState, useEffect } from 'react'

function IntervalCouterHooks() {

  const [count, setCount] = useState(0)

  const tick = () => {
    setCount(count + 1)
  }

  useEffect(() => {
    const interval = setInterval(tick, 1000)
    return () => {
      clearInterval(interval)
    }
  }, [])

  return (
    <div>
      {count}
    </div>
  )
}

export default IntervalCouterHooks

```

App.js

``` jsx
import React from 'react'

import './App.css'

import IntervalCounter from './components/11Counter'
import IntervalCounterHooks from './components/11IntervalCouterHooks'

const App = () => {
  return (
    <div className="App">
      <IntervalCounter />
      <IntervalCounterHooks />
    </div>
  )
}

export default App

```

그러나 카운터가 정상적으로 작동하지 않는 경우 효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB13TdBx7T2gK0jSZPcXXcKkpXa-425-270.gif)

빈 종속성 배열`[]`를 전달하면 구성 요소가 다시 렌더링 될 때가 아니라 마운트 될 때 후크가 한 번만 실행됩니다. 하지만 이로 인해 문제가 발생할 수 있으며`setInterval`의 콜백에서 count 값은 변경되지 않습니다. 효과가 실행되면 클로저를 생성하고 클로저에 'count'값을 저장하고 초기 값은 0이기 때문입니다. 매초 콜백은`setCount (0 + 1)`를 실행하므로`count`는 1을 초과하지 않습니다.

솔루션 1 : 여기서 useEffect의 두 번째 매개 변수를 빈 배열로 설정할 수 없지만`[count]`.

종속성 목록으로`[count]`를 지정하면이 버그를 수정할 수 있지만 변경이 발생할 때마다 타이머가 재설정됩니다. 실제로 각`setInterval`은 지워지기 전에 한 번 호출됩니다 (setTimeout과 유사). 그러나 이것은 우리가 원하는 것이 아닙니다. 이 문제를 해결하기 위해 setState의 기능 업데이트 형식을 사용할 수 있습니다. 현재 상태를 참조하지 않고 상태를 변경하는 방법을 지정할 수 있습니다. 이는 아래의 두 번째 솔루션입니다.

해결 방법 2 :

의지

``` js
setCount(count + 1)
```

에

``` js
setCount((preCount) =>  preCount + 1)
```


빈 배열은 여전히 ​​useEffect의 종속성 배열에서 사용됩니다. 여기에 설정된 카운트 값은 이전 값과 관련되어 문제도 해결됩니다. 이 시점에서`setInterval`의 콜백은 여전히 ​​1 초에 한 번 호출되지만 매번 setCount 내에서 콜백에 의해 검색되는`count`는 최신 값입니다 (콜백에서 변수 이름은 c로 지정됨).

![](https://gw.alicdn.com/tfs/TB1eq1qAeT2gK0jSZFvXXXnFXXa-433-292.gif)

### 다중 사용 효과

코드에 여러 비즈니스 로직이있는 경우 서로 다른 useEffect에 작성하고 여러 useState를 작성하여 일치시키고 그룹으로 사용하여 비즈니스 로직을 더 명확하게 만들 수 있습니다.

## 효과 후크로 데이터 가져 오기

### 간단한 데이터 액세스

이 섹션에서는 useEffect를 사용하여 데이터를 가져 오는 방법에 대해 설명합니다.이 문서에서는 axios 라이브러리 예제를 사용합니다.

https://jsonplaceholder.typicode.com/ 웹 사이트는 일부 json 데이터를 반환하는 샘플 요청을 제공합니다.

```jsx
import React, { useState, useEffect } from 'react'

import axios from 'axios'

interface postType {
  userId: number
  id: number
  title: string
  body: string
}

function FetchData() {

  const [posts, setPosts] = useState<postType[]>([])

  useEffect(() => {
    axios.get('https://jsonplaceholder.typicode.com/posts').then((res) => {
      const data: postType[] = res.data
      console.log(data)
      setPosts(data)
    }).catch((rej) => {
      console.log(rej)
    })
  }, [])

  return (
    <div>
      <ul>
        {
          posts.map((item) => (
            <li
              key={item.id}
            >
              {item.title}
            </li>
          ))
        }
      </ul>
    </div>
  )
}

export default FetchData
```

![](https://gw.alicdn.com/tfs/TB1UAWCAlr0gK0jSZFnXXbRRXXa-802-814.jpg)

ts가 useState에서 작성되는 방식에주의하십시오.

``` jsx
const [posts, setPosts] = useState<postType[]>([])
```

useEffect의 두 번째 종속 매개 변수는 빈 배열로 전달되어 useEffect가 한 번만 실행되도록합니다.

### 다른 데이터를 얻으려면 ID를 입력하세요.

``` jsx
import React, { useState, useEffect } from 'react'

import axios from 'axios'

interface postType {
  userId: number
  id: number
  title: string
  body: string
}

function FetchData() {
  const [post, setPost] = useState<postType>()
  const [id, setId] = useState('1')

  useEffect(() => {
    if (id) {
      axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`).then((res) => {
        const data: postType = res.data
        console.log(data)
        setPost(data)
      }).catch((err) => {
        console.log(err)
      })
    }
  }, [id])

  return (
    <div>
      <input
        type="text"
        value={id}
        onChange={(e) => {
          setId(e.target.value)
        }}
      />
      <div>
        {
          post && post.title
        }
      </div>
    </div>
  )
}

export default FetchData

```

![](https://gw.alicdn.com/tfs/TB1wvCvAXP7gK0jSZFjXXc5aXXa-432-292.gif)

### 버튼 클릭하면 효과가 트리거됩니다.

모니터 버튼 클릭으로 변경 트리거, 효과 실행 방법

``` jsx
import React, { useState, useEffect } from 'react'

import axios from 'axios'

interface postType {
  userId: number
  id: number
  title: string
  body: string
}

function FetchData() {
  const [post, setPost] = useState<postType>()
  const [id, setId] = useState('1')
  const [idFromBtnClick, setIdFromBtnClick] = useState('1')

  useEffect(() => {
    if (idFromBtnClick) {
      axios.get(`https://jsonplaceholder.typicode.com/posts/${idFromBtnClick}`).then((res) => {
        const data: postType = res.data
        console.log(data)
        setPost(data)
      }).catch((err) => {
        console.log(err)
      })
    }
  }, [idFromBtnClick])

  return (
    <div>
      <input
        type="text"
        value={id}
        onChange={(e) => {
          setId(e.target.value)
        }}
      />
      <button
        onClick={() => {
          setIdFromBtnClick(id)
        }}
      >Fetch Post</button>
      <div>
        {
          post && post.title
        }
      </div>
    </div>
  )
}

export default FetchData
```

![](https://gw.alicdn.com/tfs/TB1sH2tAi_1gK0jSZFqXXcpaXXa-432-292.gif)

## 요약

이 장은 useEffect가 사용되는 이유부터 시작하여 사용자가 코드 중복 및 코드 분산 문제를 해결합니다. UseEffect는 코드를 더 잘 구성 할 수 있습니다.

useEffect api의 사용에서 첫 번째 매개 변수는 효과에 의해 실행되는 내용으로 익명 함수입니다. 두 번째 매개 변수는 변경된 props 또는 상태를 관찰하여 조건부로 효과를 트리거하거나 효과가 한 번만 실행되도록 빈 배열을 전달하는 데 사용되는 배열입니다. useEffect는 구성 요소가 파괴 될 때 실행되는 익명 함수를 반환하므로 메모리 누수 위험을 효과적으로 방지 할 수 있습니다.

마지막으로 useEffect에서 데이터를 얻기 위해 요청을 시작하는 로직을 실행하는 방법을 보여주는 몇 가지 예가 제공됩니다. 다음 장에서는 useContext API에 대해 설명합니다.