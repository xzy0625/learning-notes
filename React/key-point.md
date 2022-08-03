1. jsx本质就是一个对象，在编译过程中，jsx会被React.creatElement()编译成一个对象

2. React 元素是[不可变对象](https://en.wikipedia.org/wiki/Immutable_object)。一旦被创建，你就无法更改它的子元素或者属性。一个元素就像电影的单帧：它代表了某个特定时刻的 UI。更新 UI 唯一的方式是创建一个全新的元素，并将其传入 `root.render()`。

3. 组件名称必须以大写字母开头。React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 `Welcome`。

4. **所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**对于可变的内容，应该使用State来进行代替

5. 出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用。

   因为 `this.props` 和 `this.state` 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。

   例如，此代码可能会无法更新计数器：

   ```js
   // Wrong
   this.setState({
     counter: this.state.counter + this.props.increment,
   });
   ```

   要解决这个问题，可以让 `setState()` 接收一个函数而不是一个对象。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数：

   ```js
   // Correct
   this.setState((state, props) => ({
     counter: state.counter + props.increment
   }));
   ```

   上面使用了[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，不过使用普通的函数也同样可以：

   ```js
   // Correct
   this.setState(function(state, props) {
     return {
       counter: state.counter + props.increment
     };
   });
   ```

6. 在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。你必须显式地使用 `preventDefault`。例如，传统的 HTML 中阻止表单的默认提交行为，你可以这样写：

   ```js
   <form onsubmit="console.log('You clicked submit.'); return false">
     <button type="submit">Submit</button>
   </form>
   ```

   在 React 中，可能是这样的：

   ```js
   function Form() {
     function handleSubmit(e) {
       e.preventDefault();    console.log('You clicked submit.');
     }
   
     return (
       <form onSubmit={handleSubmit}>
         <button type="submit">Submit</button>
       </form>
     );
   }
   ```

7. 请注意，[falsy 表达式](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) 会使 `&&` 后面的元素被跳过，但会返回 falsy 表达式的值。在下面示例中，render 方法的返回值是 `<div>0</div>`。

   ```js
   render() {
     const count = 0;  return (
       <div>
         {count && <h1>Messages: {count}</h1>}    </div>
     );
   }
   ```

8. key 会传递信息给 React ，但不会传递给你的组件。如果你的组件中需要使用 `key` 属性的值，请用其他属性名显式传递这个值：

   ```js
   const content = posts.map((post) =>
     <Post
       key={post.id}    
   	  id={post.id}    
       title={post.title} 
   	/>
   );
   ```

   上面例子中，`Post` 组件可以读出 `props.id`，但是不能读出 `props.key`。

9. 虽然高阶组件的约定是将所有 props 传递给被包装组件，但这对于 refs 并不适用。那是因为 `ref` 实际上并不是一个 prop - 就像 `key` 一样，它是由 React 专门处理的。如果将 ref 添加到 HOC 的返回组件中，则 ref 引用指向容器组件，而不是被包装组件。

