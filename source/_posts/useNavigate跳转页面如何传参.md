---
title: useNavigate跳转页面如何传参
date: 2024-08-31 16:10:14
categories: javascript
tags: react.js
---
# 如何使用useNavigate在跳转页面的时候同时传参？

通常在react项目中，我们很多时候都需要在页面跳转的时候带一些参数，useNavigate一般有三种带参跳转的方式，废话不多说，直接上代码：

## 1.使用search, query, state如何携带参数
```javascript
import React from 'react';
import { useNavigate, useSearchParams } from 'react-router-dom';
import { Button } from 'antd';


export default () => {
  // 先定义navigate函数，跳转做准备
  const navigate = useNavigate()
  // serach 带参数
  const searchParams = () => {
    navigate("/paramsAly?name=testName&age=34")
  }
  // 使用queryparams带参
  const queryParams = () => {
    navigate("/paramsAlyQuery/12345678")
  }

  // 使用state带参
  const stateParams = () => {
    navigate("/paramsAlyState",{ state: {name: '测试', keyWords: '传参'} })
  }

  return (
    <div>
      <div>useNavigate传参怎么弄？</div>
      <Button onClick={searchParams}>search方式传参</Button>
      <Button onClick={queryParams}>query方式传参</Button>
      <Button onClick={stateParams}>state方式传参</Button>
    </div>
   
  )
}
```
## 2.对应的路由如何写
```javascript
  // 放跳转方法的页面
  {
    path: '/routerParams',
    index: true,
    element: <RouterParams />
  },
  // 解析search参数的页面
  {
    path: '/paramsAly',
    index: true,
    element: <ParamsAly />
  },
  // 解析query参数的页面 在路由的/后面加上: + ni所需要的参数名字即可
  {
    path: '/paramsAlyQuery/:id',
    index: true,
    element: <ParamsAlyQuery />
  },
  // 解析state参数的页面
  {
    path: '/paramsAlyState',
    index: true,
    element: <ParamsAlyState />
  }
```
## 3.search参数应该如何解析
```javascript
import React from 'react';
import { useSearchParams } from 'react-router-dom';


export default () => {
  // 使用hook useSearchParams获取参数
  const [ searchP, setSearchP ] = useSearchParams()
  // serach 获取参数
  console.log("Dffgg===>>>>>", searchP.get('name'), searchP.get('age'));
  let name = searchP.get('name'), age = searchP.get('age');

  return (
    <div>
      <div>useNavigate传参怎么接？</div>
      <div>
        <span>名字：{name}</span>
        <span>年龄：{age}</span>
      </div>
    </div>
   
  )
}
```

## 4.query参数应该如何解析
```javascript
import React from 'react';
import { useParams } from 'react-router-dom';


export default () => {
  // 使用hook useParams获取参数
  const params = useParams()
  // 或者是这种
  const { id } = useParams()

  return (
    <div>
      <div>useNavigate传参怎么接？</div>
      <div>
        <span>第一种方式：{params.id}</span>
      </div>
      <div>
        <span>第二种方式：{id}</span>
      </div>
    </div>
   
  )
}
```

## 5.state参数应该如何解析
```javascript
import React from 'react';
import { useLocation } from 'react-router-dom';


export default () => {
  // 使用hook useLocation获取参数
  const location = useLocation()
  // const { state } = location;
  // 或者是这种
  const { state } = useLocation()

  return (
    <div>
      <div>useNavigate传参怎么接？</div>
      <div>
        <span>第一种方式：{location.state.name}</span>
      </div>
      <div>
        <span>第二种方式：{state.keyWords}</span>
      </div>
    </div>
   
  )
}
```

## 总结
很多时候我们需要的不同的路由，可以根据需求选择，比如写blog，查看单篇文章的时候，可以使用query传参方式，如果一些信息不宜被链接查看，可以使用state传参方式。