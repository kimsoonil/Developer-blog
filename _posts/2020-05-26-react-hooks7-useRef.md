---
layout: post
title:  "React Hooks useRef"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}

React Hooks api를 마스터하면 작업에서 더 잘 사용할 수 있고 React를 더 잘 이해할 수 있습니다. 이 시리즈에서는 초보자와 리뷰어가 사용하기 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

다음으로 컴포넌트의 Dom 노드에 직접 액세스 할 수있는 useRef 후크를 함께 배우겠습니다. 포커스를 얻기위한 입력 상자의 필요성을 예로 들어 오늘 useRef에 대해 알아 보겠습니다.




## 포커스를 얻기위한 페이지 로딩 입력의 예

FocusInput.js

``` js
import React, { useEffect, useRef} from 'react'

function FocusInput() {
  const inputRef = useRef<HTMLInputElement>(null)
  useEffect(() => {
    inputRef.current && inputRef.current.focus()
  }, [])

  return (
    <div>
      <input ref={inputRef} type="text" />
    </div>
  )
}

export default FocusInput
```

App.js

``` js
import React from 'react'
import './App.css'

import FocusInput from './components/28FocusInput'

const App = () => {
  return (
    <div className="App">
      <FocusInput />
    </div>
  )
}

export default App
```

페이지 표시 효과

![](https://gw.alicdn.com/tfs/TB1lxWlHkY2gK0jSZFgXXc5OFXa-363-125.gif)

TypeScript와 결합 할 때 먼저 제네릭을 선언해야합니다.

``` js
const inputRef = useRef<HTMLInputElement>(null)
```

동시에 사용하는 경우 공백이어야합니다.

``` js
inputRef.current && inputRef.current.focus()
```

ts와 hook의 조합에 대해서는 [TypeScript and React: Hooks](https://fettblog.eu/typescript-react/hooks/#useref) 문서를 참조하십시오.

> useRefRef는 .current속성이 들어오는 매개 변수 (initialValue)로 초기화 된 변수 객체를 반환합니다 . 반환 된 참조 개체는 구성 요소의 전체 수명주기 동안 변경되지 않은 상태로 유지됩니다.

Ref는 렌더링 메서드에서 생성 된 DOM 노드 또는 React 요소에 액세스 할 수있는 방법을 제공합니다.

일반적인 React 데이터 흐름에서 props는 부모 구성 요소가 자식 구성 요소와 상호 작용하는 유일한 방법입니다. 하위 구성 요소를 수정하려면 새 소품으로 다시 렌더링해야합니다. 그러나 경우에 따라 일반적인 데이터 흐름 외부에서 하위 구성 요소를 강제로 수정해야합니다. 수정 된 하위 구성 요소는 React 구성 요소의 인스턴스이거나 DOM 요소 일 수 있습니다. 두 경우 모두 React는 솔루션을 제공합니다.

### 다음은 심판이 적합한 몇 가지 상황입니다.

 - 포커스, 텍스트 선택 또는 미디어 재생을 관리합니다.
 - 강제 애니메이션을 트리거합니다.
 - 타사 DOM 라이브러리를 통합합니다.

선언적 구현을 ​​통해 수행 할 수있는 작업을 수행하기 위해 ref를 사용하지 마십시오. 예를 들어 Dialog 구성 요소에서 open () 및 close () 메서드가 노출되지 않도록하려면 isOpen 속성을 전달하는 것이 좋습니다.

### Refs를 과도하게 사용하지 마십시오.

먼저 앱에서 "일이 일어나도록"하기 위해 ref를 사용하는 것을 생각할 수 있습니다. 이 경우 상태 속성을 정렬해야하는 구성 요소 레이어를 신중하게 고려하십시오. 일반적으로 상위 구성 요소 수준이이 상태를 소유하도록하는 것이 더 적절하다는 것을 이해하고 싶을 것입니다. 더 많은 예를 보려면 상태 프로모션을 참조하세요.

refs 및 Dom에 대한 자세한 내용은 React 공식 웹 사이트 [Refs and the DOM](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html)을 참조하십시오.

다른 시나리오에서 useRef를 사용하는 방법에 대해 알아 보겠습니다.

## 중지 할 수있는 타이머의 예

요구 사항은 페이지에 1 초마다 자동으로 증가하는 타이머가 있고 버튼이 있으며, 클릭하면 타이머가 중지됩니다. 먼저 클래스 구성 요소를 사용하여이 요구 사항을 완료합니다.

### Class 구성요소

ClassTimer.js

``` js
import React, { Component } from 'react'

export default class ClassTimer extends Component<{}, { timer: number }> {
  interval!: number
  constructor(props: Readonly<{}>) {
    super(props)
    this.state = {
      timer: 0
    }
  }

  componentDidMount() {
    this.interval = window.setInterval(() => {
      this.setState(prevState => ({
        timer: prevState.timer + 1
      }))
    }, 1000)
  }

  componentWillUnmount() {
    clearInterval(this.interval)
  }

  render() {
    return (
      <div>
        Timer - {this.state.timer}
        <br/>
        <button
          onClick={() => {
            clearInterval(this.interval)
          }}
        >Clear Timer</button>
      </div>
    )
  }
}
```

App.js

``` js
import React from 'react'
import './App.css'

import ClassTimer from './components/29ClassTimer'

const App = () => {
  return (
    <div className="App">
      <ClassTimer />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1J31IHHr1gK0jSZFDXXb9yVXa-437-179.gif)

### Function 구성요소

HookTimer.js

``` js
import React, { useState, useEffect, useRef } from 'react'

function HookTimer() {
  const [timer, setTimer] = useState(0)

  //  @ts-ignore
  const intervalRef = useRef(null) as { current: number }

  useEffect(() => {
    intervalRef.current = window.setInterval(() => {
      setTimer(pre => pre + 1)
    }, 1000)
    return () => {
      clearInterval(intervalRef.current)
    }
  }, [])
  return (
    <div>
      HookTimer - {timer}
      <br />
      <button
        onClick={() => {
          clearInterval(intervalRef.current)
        }}
      >Clear Hook Timer</button>
    </div>
  )
}

export default HookTimer
```

App.js

``` js
import React from 'react'
import './App.css'

import ClassTimer from './components/29ClassTimer'
import HookTimer from './components/29HookTimer'

const App = () => {
  return (
    <div className="App">
      <ClassTimer />
      <hr />
      <HookTimer />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1v51OHQL0gK0jSZFxXXXWHVXa-437-227.gif)

이것은 useRef의 두 번째 사용법으로, 변수를 저장하기위한 일반 컨테이너를 만드는 데 사용할 수 있습니다.

## 小结

이 장에서 우리는 useRef의 두 가지 사용법을 배웠습니다. 하나는 Dom 노드에 액세스 할 수 있도록하는 것이고 다른 하나는 변수를 캐시하는 컨테이너가되는 것입니다. 두 번째 사용은 상대적으로 드물고 더 많은주의가 필요합니다. 유사한 시나리오에서 사용해 볼 수 있습니다.


