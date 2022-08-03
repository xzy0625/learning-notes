```react
import React, { Component } from 'react';
import _ from 'lodash';
import myLodash from 'mylodash'
import './App.css';

// 纯展示组件
const Cat = ({ data, user }) => {
  const { title, name } = data;
  return (
    <div>
      <h1>{ title }</h1>
      <p>{ name }</p>
      <p>性别 { user.gender }, 年龄 { user.age }</p>
    </div>
  )
}


// 高阶组件
const WithUser = InnerComponent => {
  const user = {
    age: 18,
    gender: 'famel',
  }
  
  // 传入数据，并将原来的props一并传入，如果是class组件的话可以不用返回函数，直接在render中返回组件即可
  return props => <InnerComponent user={user} {...props} />
}

const WithCat = WithUser(Cat);

// reder porps
class RenderComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      title: '测试',
      name: '123',
    }
  }

  render() {
    return (
      // 通过props传进来的渲染函数进行渲染，本身只进行数据的处理,虽说叫render props，其实我们并不需要一定是传入一个叫做render的props,我们也可以传入其他函数名字，随便取，反正就是一个props而已，react万物都可以是props
      this.props.render(this.state)
    );
  }
}

class App extends React.Component{
  render() {
    return (
        <RenderComponent render={(data) => <WithCat data={data}/>}/>
    );
  }
}
  
export default App;

```

高阶组件很Render Props本质上都是为了提高组件的复用率，让组件只用专注于展示，数据都从render Props或者高阶组件中传进去。