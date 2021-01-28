---
layout: post
title:  "React Hooks custom Hook"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}

React Hooks api를 마스터하면 작업에서 더 잘 사용할 수 있고 React를 더 잘 이해할 수 있습니다. 이 시리즈에서는 초보자와 리뷰어가 사용하기 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

지금까지 많은 공식 후크 API를 학습 한 후 자체 후크 중 일부를 만들 수도 있으며, 심지어 공식적으로도 개발자가 쉽게 재사용 할 수 있도록 구성 요소 논리를 사용자 지정 후크로 추상화하도록 권장하고 있습니다.

사용자 정의 후크는 이름이 "use"로 시작하는 함수이며 다른 후크는 함수 내에서 호출 될 수 있습니다.

Hook을 커스터마이징함으로써 컴포넌트 로직을 재사용 가능한 함수로 추출 할 수 있습니다.






## useDocumentTitle 

### function 

카운터를 만들고 싶습니다. 카운터의 값이 변경된 후 페이지의 제목을 변경하려고합니다.

DocTitleOne.js

``` js
import React, { useState, useEffect } from 'react'

function DocTitleOne() {
  const [count, setCount] = useState(0)
  useEffect(() => {
    document.title = `Count - ${count}`
  }, [count])
  return (
    <div>
      <button
        onClick={() => {
          setCount(count + 1)
        }}
      >Count - {count}</button>
    </div>
  )
}

export default DocTitleOne

```

App.js

``` js
import React from 'react'
import './App.css'

import DocTitleOne from './components/31DocTitleOne'

const App = () => {
  return (
    <div className="App">
      <DocTitleOne />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1ilw2HUY1gK0jSZFMXXaWcVXa-407-221.gif)

실행하는 데 문제가 없으며 다른 수요 증가가 있습니다. 즉, 다른 구성 요소에서 페이지 제목을 변경할 수 있으며 새 구성 요소를 만듭니다.

DocTitleTwo.js

``` js
import React, { useState, useEffect } from 'react'

function DocTitleTwo() {
  const [count, setCount] = useState(0)
  useEffect(() => {
    document.title = `Count - ${count}`
  }, [count])
  return (
    <div>
      <button
        onClick={() => {
          setCount(count + 1)
        }}
      >Count - {count}</button>
    </div>
  )
}

export default DocTitleTwo
```

App.js

``` js
import React from 'react'
import './App.css'

import DocTitleOne from './components/31DocTitleOne'
import DocTitleTwo from './components/31DocTitleTwo'

const App = () => {
  return (
    <div className="App">
      <DocTitleOne />
      <DocTitleTwo />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1sNg1HUz1gK0jSZLeXXb9kVXa-407-221.gif)

코드를 되돌아 보면 DocTitleTwo는 분명히 DocTitleOne의 코드를 반복합니다. 페이지 제목을 수정해야하는 구성 요소가 10 개 있다고 가정 해보면 이러한 코드를 반복하고 싶지 않을 것입니다. 이때 Hook을 커스터마이즈해야합니다.

이 예에서는 페이지 제목을 설정하는 사용자 지정 후크를 만들 수 있습니다. 그런 다음 다른 구성 요소에서이 사용자 지정 후크를 사용합니다.

### useDocumentTitle hook 추상화

useDocumentTitle.js

``` js
import { useEffect } from 'react'

function useDocumentTitle(count: number) {
  useEffect(() => {
    document.title = `Count - ${count}`
  }, [count])
}

export default useDocumentTitle
```

DocTitleOne.js

``` js
import React, { useState } from 'react'
import useDocumentTitle from './hooks/useDocumentTitle'

function DocTitleOne() {
  const [count, setCount] = useState(0)
  useDocumentTitle(count)
  return (
    <div>
      <button
        onClick={() => {
          setCount(count + 1)
        }}
      >Count - {count}</button>
    </div>
  )
}

export default DocTitleOne
```

DocTitleTwo.js

``` js
import React, { useState } from 'react'
import useDocumentTitle from './hooks/useDocumentTitle'

function DocTitleTwo() {
  const [count, setCount] = useState(0)
  useDocumentTitle(count)
  return (
    <div>
      <button
        onClick={() => {
          setCount(count + 1)
        }}
      >Count - {count}</button>
    </div>
  )
}

export default DocTitleTwo
```

App.js

``` js
import React from 'react'
import './App.css'

import DocTitleOne from './components/31DocTitleOne'
import DocTitleTwo from './components/31DocTitleTwo'

const App = () => {
  return (
    <div className="App">
      <DocTitleOne />
      <DocTitleTwo />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1sNg1HUz1gK0jSZLeXXb9kVXa-407-221.gif)

코드를 검토해 보겠습니다.

DocTitleOne에서는 우리가 정의한 useDocumentTitle이 도입되고 count 상태의 값이 전달됩니다. useDocumentTitle에서 코드를 실행하고 페이지 제목의 초기 값을 0으로 설정 한 다음 DocTitleOne js 부분을 계속 렌더링합니다. 버튼을 클릭하면 개수가 1이되어 DocTitleOne의 렌더러가 트리거되고 useDocumentTitle의 입력 매개 변수도 1이되고 페이지 제목이 1이됩니다.

## useCounter 

### 중복문구

CounterOne.js

``` js
import React, {useState} from 'react'

function CounterOne() {
  const [count, setCount] = useState(0)
  const increment = () => {
    setCount(prevCount => prevCount + 1)
  }
  const decrement = () => {
    setCount(prevCount => prevCount - 1)
  }
  const reset = () => {
    setCount(0)
  }
  return (
    <div>
      <h2>Count - {count}</h2>
      <button onClick={increment}>increment</button>
      <button onClick={decrement}>decrement</button>
      <button onClick={reset}>reset</button>
    </div>
  )
}

export default CounterOne
```

CounterTwo.js

``` js
import React, {useState} from 'react'

function CounterTwo() {
  const [count, setCount] = useState(0)
  const increment = () => {
    setCount(prevCount => prevCount + 1)
  }
  const decrement = () => {
    setCount(prevCount => prevCount - 1)
  }
  const reset = () => {
    setCount(0)
  }
  return (
    <div>
      <h2>Count - {count}</h2>
      <button onClick={increment}>increment</button>
      <button onClick={decrement}>decrement</button>
      <button onClick={reset}>reset</button>
    </div>
  )
}

export default CounterTwo
```

App.js

``` js
import React from 'react'
import './App.css'
import CounterOne from './components/32CounterOne'
import CounterTwo from './components/32CounterTwo'

const App = () => {
  return (
    <div className="App">
      <CounterOne />
      <CounterTwo />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.


![](https://gw.alicdn.com/tfs/TB1Kf78HQL0gK0jSZFxXXXWHVXa-407-347.gif)

같은 문제에 대해 반복되는 코드가 많이 있는데, 다음으로 최적화를 위해 사용자 지정 후크를 사용하는 방법을 살펴 보겠습니다.

### useCounter 추상화

useCounter.js

``` js
import { useState } from 'react'

function useCounter() {
  const [count, setCount] = useState(0)
  const increment = () => {
    setCount(prevCount => prevCount + 1)
  }
  const decrement = () => {
    setCount(prevCount => prevCount - 1)
  }
  const reset = () => {
    setCount(0)
  }
  return [count, increment, decrement, reset]
}

export default useCounter
```

CounterOne.js

``` js
import React from 'react'
import useCounter from './hooks/useCounter'

function CounterOne() {
  const [count, increment, decrement, reset] = useCounter()
  return (
    <div>
      <h2>Count - {count}</h2>
      <button onClick={increment}>increment</button>
      <button onClick={decrement}>decrement</button>
      <button onClick={reset}>reset</button>
    </div>
  )
}

export default CounterOne
```

CounterTwo.js

``` js
import React from 'react'
import useCounter from './hooks/useCounter'

function CounterTwo() {
  const [count, increment, decrement, reset] = useCounter()
  return (
    <div>
      <h2>Count - {count}</h2>
      <button onClick={increment}>increment</button>
      <button onClick={decrement}>decrement</button>
      <button onClick={reset}>reset</button>
    </div>
  )
}

export default CounterTwo
```

App.js
``` js
import React from 'react'
import './App.css'
import CounterOne from './components/32CounterOne'
import CounterTwo from './components/32CounterTwo'

const App = () => {
  return (
    <div className="App">
      <CounterOne />
      <CounterTwo />
    </div>
  )
}

export default App
```

페이지 표시는 여전히 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1Kf78HQL0gK0jSZFxXXXWHVXa-407-347.gif)

현재 코드 구조가 더 나은 것을 알 수 있습니다. 다음과 같이 useCounter에서 카운터의 초기 값을 설정할 수도 있습니다.

useCounter.js

``` js
import { useState } from 'react'

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue)
  const increment = () => {
    setCount(prevCount => prevCount + 1)
  }
  const decrement = () => {
    setCount(prevCount => prevCount - 1)
  }
  const reset = () => {
    setCount(initialValue)
  }
  return [count, increment, decrement, reset]
}

export default useCounter
```

사용하면 해당 초기 값을 전달할 수 있습니다.

``` js
const [count, increment, decrement, reset] = useCounter(10)
```

사용할 때 해당 매개 변수도 추가 할 수 있습니다.

``` js
import { useState } from 'react'

function useCounter(initialValue = 0, value = 1) {
  const [count, setCount] = useState(initialValue)
  const increment = () => {
    setCount(prevCount => prevCount + value)
  }
  const decrement = () => {
    setCount(prevCount => prevCount - value)
  }
  const reset = () => {
    setCount(initialValue)
  }
  return [count, increment, decrement, reset]
}

export default useCounter
```

사용할 때 해당 매개 변수도 추가 할 수 있습니다.

``` js
const [count, increment, decrement, reset] = useCounter(10, 5)
```

## useInput 

예제는 간단한 양식이며 사용자가 이름을 입력 할 수 있습니다.

### function 일반쓰기

UserForm.js

``` js
import React, { useState, FormEvent } from 'react'

function UserForm() {
  const [firstName, setFirstName] = useState('')
  const [lastName, setLastName] = useState('')
  const submitHandler = (e: FormEvent) => {
    e.preventDefault()
    console.log(`Hello ${firstName} ${lastName}`)
  }
  return (
    <div>
      <form onSubmit={submitHandler}>
        <div>
          <label htmlFor="">First name</label>
          <input
            type="text"
            value={firstName}
            onChange={(e) => {
              setFirstName(e.target.value)
            }}
          />
        </div>
        <div>
          <label htmlFor="">Last name</label>
          <input
            type="text"
            value={lastName}
            onChange={(e) => {
              setLastName(e.target.value)
            }}
          />
        </div>
        <button>submit</button>
      </form>
    </div>
  )
}

export default UserForm
```

App.js

``` js
import React from 'react'
import './App.css'

import UserForm from './components/33UserForm'

const App = () => {
  return (
    <div className="App">
      <UserForm />
    </div>
  )
}

export default App
```

![](https://gw.alicdn.com/tfs/TB105U6HQL0gK0jSZFtXXXQCXXa-696-241.gif)

### 抽象出 useInput hook

useInput.js

``` js
import { useState } from 'react'

function useInput(initialValue: string) {
  const [value, setValue] = useState(initialValue)
  const reset = () => {
    setValue(initialValue)
  }
  const bind = {
    value,
    onChange(e: any) {
      setValue(e.target.value)
    }
  }
  return [value, bind, reset]
}

export default useInput
```

UserForm.js

``` js
import React, { FormEvent } from 'react'
import useInput from './hooks/useInput'

function UserForm() {

  const [firstName, bindFirstName, resetFirstName] = useInput('')
  const [lastName, bindLastName, resetLastName] = useInput('')

  const submitHandler = (e: FormEvent) => {
    e.preventDefault()
    console.log(`Hello ${firstName} ${lastName}`)
    // @ts-ignore
    resetFirstName()
    // @ts-ignore
    resetLastName()
  }
  return (
    <div>
      <form onSubmit={submitHandler}>
        <div>
          <label htmlFor="">First name</label>
          <input
            type="text"
            {...bindFirstName}
          />
        </div>
        <div>
          <label htmlFor="">Last name</label>
          <input
            type="text"
            {...bindLastName}
          />
        </div>
        <button>submit</button>
      </form>
    </div>
  )
}

export default UserForm
```

페이지 표시

![](https://gw.alicdn.com/tfs/TB1zXxbH7T2gK0jSZFkXXcIQFXa-696-241.gif)

## 정리

이 장에서 우리는 주로 커스텀 후크에 대해 배웠고 코드를 추상화하고 재사용하는 방법을 배우는 데 도움이되는 3 가지 예제를 제공했습니다. 커뮤니티에는 여전히 자신의 맞춤 후크를 작성한 많은 사람들이 있으며, 가서 배울 수도 있습니다. 모든 사람이 자신 만의 고유 한 후크를 만들도록 권장됩니다.

이 시점에서이 시리즈는 끝났습니다. 최선을 다하고 모두가 무언가를 배울 수 있기를 바랍니다.
