---
layout: post
title:  "React Hooks useCallback"
categories: JavaScript
tags: React hooks
author: KSI
---

* content
{:toc}

React Hooks api를 마스터하면 작업에서 더 잘 사용할 수 있고 React를 더 잘 이해할 수 있습니다. 이 시리즈에서는 초보자와 리뷰어가 사용하기 매우 쉬운 많은 예제 코드와 효과 데모를 사용합니다.

useCallback에 대해 자세히 알아보기 전에 성능 최적화와 관련된 내용을 검토해 보겠습니다. 그러면 useCallback이 무엇인지, 왜 사용하는지, 사용 방법을 이해하는 데 도움이 될 것입니다.

구성 요소가 여러 번 재사용되는 코드 시나리오부터 시작하겠습니다.



## 구성 요소가 여러 번 재사용되는 시나리오

컴포넌트 트리 구조는 다음과 같습니다. ParentWrap에는 Title 구성 요소, Count 구성 요소 2 번, Button 구성 요소가 2 번 포함됩니다.

버튼을 클릭하면 해당 카운트가 각각 증가합니다.

![](https://gw.alicdn.com/tfs/TB1e0AVGpT7gK0jSZFpXXaTkpXa-1270-595.png)

App.js

``` js
import React from 'react'
import './App.css'

import ParentComponent from './components/26ParentComponenet'

const App = () => {
  return (
    <div className="App">
      <ParentComponent />
    </div>
  )
}

export default App
```

ParentComponent.js

``` js
import React, { useState } from 'react'
import Title from './26Title'
import Count from './26Count'
import Button from './26Button'

function ParentComponenet() {
  const [age, setAge] = useState(29)
  const [salary, setSalary] = useState(50000)
  const incrementAge = () => {
    setAge(age + 1)
  }
  const incrementSalary = () => {
    setSalary(salary + 1000)
  }
  return (
    <div>
      <Title />
      <Count
        text="Age"
        count={age}
      />
      <Button
        handleClick={incrementAge}
      >Increment age</Button>
      <Count
        text="Salary"
        count={salary}
      />
      <Button
        handleClick={incrementSalary}
      >Increment salary</Button>
    </div>
  )
}

export default ParentComponenet
```

Title.js

``` js
import React from 'react'

function Title() {
  console.log('Rendering Title')
  return (
    <h2>useCallback</h2>
  )
}

export default Title
```

Count.js

``` js
import React from 'react'

function Count(props: {
  text: string
  count: number
}) {
  console.log(`Rendering ${props.text}`)
  return (
    <div>
      {props.text} - {props.count}
    </div>
  )
}

export default Count
```

Button.js

``` js
import React from 'react'

function Button(props: {
  handleClick: () => void
  children: string
}) {
  console.log('Rendering button', props.children)
  return (
    <button onClick={props.handleClick}>
      {props.children}
    </button>
  )
}

export default Button
```

물론 모든 jsx 코드를 함께 작성할 수 있습니다. 이러한 방식으로 코드를 구성하는 주된 이유는 모든 사람들이 반응에서 성능 최적화를 이해하고 성능 최적화를 위해 useCallback API를 사용하는 방법을 이해하도록하기 위해서입니다.

페이지 표시 효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB1PhFeGO_1gK0jSZFqXXcpaXXa-702-606.gif)

주의해야 할 점은 콘솔에서 볼 수있는 성능 문제입니다. 클릭 할 때마다 다음 로그가 표시됩니다.

```
Rendering Title
Rendering Age
Rendering button Increment age
Rendering Salary
Rendering button Increment salary
```

각 상태 변경은 모든 구성 요소의 렌더러를 트리거합니다.이 예제는 비교적 간단하지만 나중에 20, 30 또는 50 개의 구성 요소 렌더러가 발생할 경우 성능 문제를 고려해야합니다. 이 예에서 최적화하는 방법에 대해 이야기하겠습니다.

## 使用 React.memo 优化

물론이 예제에서는 나이를 늘리기 위해 버튼을 클릭하면 나이에 대한 Count와 Button 만 렌더링되고 다른 컴포넌트는 렌더링되지 않기를 바랍니다. 급여를 올리기 위해 클릭 할 때도 마찬가지입니다. . 어떻게 할 수 있습니까? 대답은 React.memo입니다.

Title.tsx, Count.tsx, Button.tsx를 추가 React.memo()하고 코드는 다음과 같습니다.

Title.js

``` js
import React from 'react'

function Title() {
  console.log('Rendering Title')
  return (
    <h2>useCallback</h2>
  )
}

export default React.memo(Title)

```

Count.js

``` js
import React from 'react'

function Count(props: {
  text: string
  count: number
}) {
  console.log(`Rendering ${props.text}`)
  return (
    <div>
      {props.text} - {props.count}
    </div>
  )
}

export default React.memo(Count)
```

Button.js

``` js
import React from 'react'

function Button(props: {
  handleClick: () => void
  children: string
}) {
  console.log('Rendering button', props.children)
  return (
    <button onClick={props.handleClick}>
      {props.children}
    </button>
  )
}

export default React.memo(Button)
```

효과는 다음과 같습니다.

![](https://gw.alicdn.com/tfs/TB18aNkGKL2gK0jSZFmXXc7iXXa-702-606.gif)

> React.memo는 고수준 구성 요소입니다. React.PureComponent와 매우 유사하지만 클래스 구성 요소가 아닌 기능 구성 요소에만 적용됩니다.

``` js
const MyComponent = React.memo(function MyComponent(props) {
  /* props 렌더링 사용하기 */
});
```

> 기능적 구성 요소가 동일한 props에서 동일한 결과를 렌더링하는 경우 React.memo로 래핑하여 호출하여 구성 요소의 렌더링 결과를 기억하여 구성 요소의 성능을 향상시킬 수 있습니다. 즉,이 경우 React는 컴포넌트 렌더링 작업을 건너 뛰고 가장 최근 렌더링 결과를 직접 재사용합니다.
> 
> React.memo는 소품의 변경 사항 만 확인합니다. 기능 구성 요소가 React.memo에 의해 래핑되고 그 구현에 useState 또는 useContext Hook이있는 경우 컨텍스트가 변경 될 때 여전히 다시 렌더링됩니다.
>
> 기본적으로 복잡한 객체에 대해서만 얕은 비교를 수행합니다. 비교 프로세스를 제어하려면 두 번째 매개 변수를 통해 사용자 정의 비교 함수를 전달하십시오.

``` js
function MyComponent(props) {
  /* props 렌더링 사용하기 */
}
function areEqual(prevProps, nextProps) {
  /*
  렌더에 nextProps를 불러오는 방법의 반환과 결과
  prevProps를 렌더로 불러오는 방법의 결과 일치true로 돌아가면
  그렇지 않으면 false로 돌아가기
  */
}
export default React.memo(MyComponent, areEqual);
```

> 이 방법은 성능 최적화 방법으로 만 존재합니다. 그러나 렌더링을 "방지"하는 데 의존하지 마십시오. 버그가 발생할 수 있습니다.

그러나 React.memo를 사용한 후 버튼을 클릭하여 나이를 늘리면 로그가

```
Rendering Age
Rendering button Increment age
Rendering button Increment salary
```

여전히 관련없는 렌더러가 `Rendering button Increment salary`있습니다. 분석해 보겠습니다.

ParentComponenet.tsx에서 Increment age 버튼을 클릭하면 상태가 변경되고 ParentComponenet이 렌더링을 수행하는 것을 볼 수 있습니다. `<Title />` 들어오는 속성이 없으면 React.memo는 렌더러가 필요하지 않다고 판단했지만 Increment salary 버튼의 속성 incrementSalary 메서드가 실제로 다시 생성되어 Button이 전달한 props가 변경되었으므로 React.memo는이를 방지하지 못했습니다. 렌더러. 급여 증가 버튼을 클릭하여 발생하는 현상은 동일합니다. 그래서 그것을 해결하는 방법? 대답은 useCallback 후크를 사용하는 것입니다.

## useCallback

### useCallback란

``` js
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

> 메모 된 콜백 함수를 반환합니다.
>
> 인라인 콜백 함수와 종속성 배열을 매개 변수 useCallback로 전달하면 콜백 함수의 메모 화 된 버전이 반환됩니다. 콜백 함수는 종속성이 변경 될 때만 업데이트됩니다. 최적화 된 하위 구성 요소에 콜백 함수를 전달하고 불필요한 렌더링 (예 : shouldComponentUpdate)을 피하기 위해 참조 동등성을 사용할 때 매우 유용합니다.

우리의 경우와 유사하게, 우리의 useCallback `incrementSalary()`은 급여가 변경되지 않은 경우 캐시에 직접 반환 값을 캐시에, 급여가 변경되면 useCallback 종속 변경 사항을 캐시하므로 새로운 접근 방식이 반환됩니다.

이는 불필요한 렌더링 문제를 피하기 위해 특정 변수에만 의존하는 하위 구성 요소를 해결하는 데 도움이 될 수 있습니다.

### useCallback 사용 방법

다음과 같이 진행하십시오.

1. import useCallback
2. call useCallback

useCallback을 사용하여 다음과 같이 ParentComponenet.tsx에서 incrementAge 및 incrementSalary를 다시 작성합니다.

``` js
const incrementAge = useCallback(
  () => {
    setAge(age + 1)
  },
  [age],
)

const incrementSalary = useCallback(
  () => {
    setSalary(salary + 1000)
  },
  [salary],
)
```

![](https://gw.alicdn.com/tfs/TB1iQVkGLb2gK0jSZK9XXaEgFXa-702-411.gif)

보시다시피 목표가 달성되었습니다. 연령 증가를 클릭하면 급여 버튼 컴포넌트에 렌더러가 없습니다. 지금까지 모든 성능 최적화를 완료했습니다.



## 小结

이 장에서는 컴포넌트가 여러 번 재사용되는 예제에서 시작하여 단계별로 성능을 최적화하며, 핵심은 불필요한 렌더러를 방지하는 것입니다.

소품이나 상태에 변화가 없을 때 컴포넌트 렌더러를 방지하기 위해 React.memo를 사용하는 방법을 배웁니다.

useCallback이 무엇인지, useCallback을 사용하여 메서드를 캐시하는 방법, 몇 가지 변수 변경에만 의존하여 업데이트하고, 자식 구성 요소에 전달 된 소품이 매번 업데이트되는 것을 방지하고, 궁극적으로 자식 구성 요소의 불필요한 렌더러를 방지하는 방법을 배웠습니다.
