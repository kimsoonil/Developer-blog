---
layout: post
title:  "React Hooks useMemo"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}

React Hooks api를 마스터하면 작업에서 더 잘 사용할 수 있고 React를 더 잘 이해할 수 있습니다. 이 시리즈에서는 초보자와 리뷰어가 사용하기 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

지난 장에서 성능 최적화를위한 useCallback에 대해 배웠습니다. 성능 최적화를위한 또 다른 후크 API 인 useMemo가 있습니다. 예제를 살펴 보겠습니다.





## 카운터

여전히 카운터의 예입니다. 2 개의 카운터를 생성하여 전류가 홀수인지 짝수인지 구분할 수 있습니다. 버튼을 클릭했을 때 많은 양의 계산 로직이 포함되어 성능에 영향을 미치는 것을 시뮬레이션하기 위해 쓸모없는 계산 성능을 악화시키기 위해 짝수를 판단하는 방법에 논리가 추가되었습니다. 아래와 같이 코드 쇼

Counter.js

``` js
import React, { useState } from 'react'

function Counter() {
  const [counterOne, setCounterOne] = useState(0)
  const [counterTwo, setCounterTwo] = useState(0)

  const incrementOne = () => {
    setCounterOne(counterOne + 1)
  }

  const incrementTwo = () => {
    setCounterTwo(counterTwo + 1)
  }

  const isEven = () => {
    let i = 0
    while (i < 1000000000) i += 1
    return counterOne % 2 === 0
  }

  return (
    <div>
      <button
        onClick={incrementOne}
      >Count One = {counterOne}</button>
      <span>
        {
          isEven() ? 'even' : 'odd'
        }
      </span>
      <br />
      <button
        onClick={incrementTwo}
      >Count Two = {counterTwo}</button>
    </div>
  )
}

export default Counter
```

App.js

``` js
import React from 'react'
import './App.css'

import Counter from './components/27.Counter'

const App = () => {
  return (
    <div className="App">
      <Counter />
    </div>
  )
}

export default App
```

페이지는 다음과 같이 표시됩니다.

![](https://gw.alicdn.com/tfs/TB1r2l1GND1gK0jSZFKXXcJrVXa-702-286.gif)

첫 번째 버튼을 클릭하면 짝수를 판단하는 논리에 많은 계산 논리가 포함되어 있기 때문에 지연 시간이 더 길다는 것을 알았습니다. 그러나 두 번째 버튼을 클릭하면 지연 시간도 길어집니다! 이건 이상해.

이는 상태가 업데이트 될 때마다 구성 요소가 렌더링되고 isEven이 실행되기 때문입니다. 이것이 두 번째 버튼을 클릭 할 때 멈추는 이유입니다. 우리는 React를 최적화하고 불필요한 계산, 특히 복잡한 계산을하지 않도록 지시해야합니다.

이 예에서는 두 번째 버튼을 클릭 할 때 isEven 메서드를 실행하지 않도록 React에 지시하고 싶습니다. 이때 useMemo 후크가 필요합니다.

## useMemo

### 최적화

사용법은 useCallback와 유사합니다.

``` js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
>메모 된 값을 반환합니다. "create"함수와 종속성 배열을 매개 변수로 전달 useMemo하면 종속성이 변경 될 때만 메모 된 값을 다시 계산합니다. 이 최적화는 렌더링 할 때마다 값  비싼 계산을 피하는 데 도움이됩니다.
>
> useMemo함수 전달 은 렌더링 중에 수행됩니다. useEffect대신 해당 범주에 속하는 이러한 작업의 부작용과 같은 내부 실행 및 렌더링 작업의 기능과 관련이 없습니다 useMemo.
>
> 종속성 배열이 제공되지 않으면 useMemo렌더링 될 때마다 새 값이 계산됩니다.
>
> **useMemo를 성능 최적화 수단으로 사용할 수 있지만 의미 론적 보장으로 사용하지 마십시오.** 향후 React는 이전에 메모 된 값 중 일부를 "잊고"화면 외부 구성 요소를위한 메모리 해제와 같은 다음 렌더링에서 다시 계산할 수 있습니다. 먼저 useMemo없이 실행할 수있는 코드를 작성한 다음 코드에 useMemo를 추가하여 성능을 최적화합니다.

首先引入 useMemo

``` js
import React, { useState, useMemo } from 'react'
```

然后将 isEven 方法使用 useMemo 改写，返回值赋给 isEven

``` js
const isEven = useMemo(() => {
  let i = 0
  while (i < 1000000000) i += 1
  return counterOne % 2 === 0
}, [counterOne])
```

最后记得修改 isEven 使用的地方，已经从一个方法变为了一个变量

``` js
{
  isEven ? 'even' : 'odd'
}
```

完整代码如下

Counter.js

``` js
import React, { useState, useMemo } from 'react'

function Counter() {
  const [counterOne, setCounterOne] = useState(0)
  const [counterTwo, setCounterTwo] = useState(0)

  const incrementOne = () => {
    setCounterOne(counterOne + 1)
  }

  const incrementTwo = () => {
    setCounterTwo(counterTwo + 1)
  }

  const isEven = useMemo(() => {
    let i = 0
    while (i < 1000000000) i += 1
    return counterOne % 2 === 0
  }, [counterOne])

  return (
    <div>
      <button
        onClick={incrementOne}
      >Count One = {counterOne}</button>
      <span>
        {
          isEven ? 'even' : 'odd'
        }
      </span>
      <br />
      <button
        onClick={incrementTwo}
      >Count Two = {counterTwo}</button>
    </div>
  )
}

export default Counter
```

효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1Oz_bb5cKOu4jSZKbXXc19XXa-702-286.gif)

두 번째 버튼을 클릭하면 지연이 발생하지 않습니다. 이는 useMemo가 counterOne 변수에만 의존하기 때문입니다. 두 번째 버튼을 클릭하면 isEven이 캐시 된 값을 읽고 IsEven이 있는지 여부를 다시 계산할 필요가 없습니다. 짝수.

### useMemo와 useCallback의 차이점

useCallback은 함수 자체를 캐시하고 useMemo는 함수의 반환 값을 캐시합니다.

## 정리

이 장에서는 예제를 통해 성능 최적화에서 useMemo의 역할을 설명합니다. 함수의 반환 값을 캐싱함으로써 불필요한 호출을 피하고 컴포넌트 렌더러를 피할 수 있습니다.

마지막으로 useMemo와 useCallback의 차이가 분석됩니다. 즉, useMemo는 함수의 반환 값을 캐시하고 useCallback은 함수 자체를 캐시합니다. 이 두 API는 모두 성능 최적화 방법입니다.
