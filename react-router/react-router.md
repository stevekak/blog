### [react-router](https://github.com/ReactTraining/react-router/tree/v5.1.2) 源码浅析

通过一个简单的demo来一步一步揭开react-router的神秘面纱

``` javascript
  <BrowserRouter>
    <Switch>
      <Route exact path="/">
        <Component0 />
      </Route>
      <Route path="/c1">
        <Component1 />
      </Route>
      <Redirect to="/" />
    </Switch>
  </BrowserRouter>
```
##### Router组件 -- 路由监听器

通过[BrowserRouter](https://github.com/ReactTraining/react-router/blob/v5.1.2/packages/react-router-dom/modules/BrowserRouter.js)的代码可以看到它做的事情很简单，只是创建了history并通过props传递给了[Router](https://github.com/ReactTraining/react-router/blob/v5.1.2/packages/react-router/modules/Router.js)组件。

``` javascript

// BrowserRouter

class BrowserRouter extends React.Component {
  history = createHistory(this.props);

  render() {
    return <Router history={this.history} children={this.props.children} />;
  }
}
```
在[Router](https://github.com/ReactTraining/react-router/blob/v5.1.2/packages/react-router/modules/Router.js)中有一个小技巧，就是 ==this._isMounted==，为了防止子组件中有一些重定向的操作，只有在父组件挂载之后，才会 ==真正== 开始对路由进行监听，防止在初次渲染时页面可能会出现的抖动现象。当路由发生变化时 Router 会调用setState方法更新在[Context](https://github.com/StringEpsilon/mini-create-react-context)中location的状态。由于Router所在的位置是根路由，所以在静态方法computeRootMatch中 isExact 的判断条件是 pathname === "/"。

``` javascript

// Router

class Router extends React.Component {
  static computeRootMatch(pathname) {
    return { path: "/", url: "/", params: {}, isExact: pathname === "/" };
  }

  constructor(props) {
    super(props);

    this.state = {
      location: props.history.location
    };
    this._isMounted = false;
    this._pendingLocation = null;

    if (!props.staticContext) {
      this.unlisten = props.history.listen(location => {
        if (this._isMounted) {
          this.setState({ location });
        } else {
          this._pendingLocation = location;
        }
      });
    }
  }

  componentDidMount() {
    this._isMounted = true;

    if (this._pendingLocation) {
      this.setState({ location: this._pendingLocation });
    }
  }

  componentWillUnmount() {
    if (this.unlisten) this.unlisten();
  }

  render() {
    return (
      <RouterContext.Provider
        children={this.props.children || null}
        value={{
          history: this.props.history,
          location: this.state.location,
          match: Router.computeRootMatch(this.state.location.pathname),
          staticContext: this.props.staticContext
        }}
      />
    );
  }
}
```
##### Switch && Route -- 渲染页面

在[Switch](https://github.com/ReactTraining/react-router/blob/v5.1.2/packages/react-router/modules/Switch.js)组件中，会通过React.Children.forEach对子组件进行了遍历，根据child元素的path或from与当前history的路由做对比，当第一次匹配成功后，通过 ==React.cloneElement== 返回一个当前元素的拷贝。

[Route](https://github.com/ReactTraining/react-router/blob/v5.1.2/packages/react-router/modules/Route.js)组件会在props.match为真值的情况下，按照 children > component > render 的权重去渲染元素。

当命中了Redirect时，先根据props找到重定向对应的路由再根据props.push去判断使用history.push 还是history.replace方法去刷新路由，因为路由发生了变化，会触发context的更新，又会重新计算一遍上述流程

##### 总结

React-Router整个的实现流程其实很简单，只是通过Context做了一个全局的状态管理，但是里边有一些函数和组件设计的十分巧妙，值得借鉴。 

谢谢阅读，文章中若存在错误之处，还请及时指正。