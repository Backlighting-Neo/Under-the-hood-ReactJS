## 简介

### 首先说明图例


[![](../images/intro/all-page-stack-reconciler-25-scale.jpg)](../images/intro/all-page-stack-reconciler.svg)

<em>简介.0 全图 (点击看大图)</em>

恩……我知道，这个图第一次看起来显得非常复杂，但事实上，它只描述了两个流程：挂载(mount)和更新(update)。我跳过了卸载(unmount)因为它可以看做挂载的一种逆过程。所以，我简化了图例。即使是这样，**这张图上并没有100%的**的匹配所有React源代码，但是主要的结构都描述清楚了。这里大概描述了60%的代码，剩下的部分并没有什么价值，所以我为了简化忽略了他们。

你应该已经注意到每个图例上有不同的颜色，每个逻辑块（看形状）的颜色取决于他父模块的颜色，比如说，‘模块A’是‘模块B’调用的，如果模块B是红色的，那么模块A就也是红色的。下面列出了所有模块对应的文件的颜色：

[![](https://rawgit.com/Bogdan-Lyashenko/Under-the-hood-ReactJS/7c2372e1/stack/images/intro/modules-src-path.svg)](https://rawgit.com/Bogdan-Lyashenko/Under-the-hood-ReactJS/7c2372e1/stack/images/intro/modules-src-path.svg)

<em>简介.1 模块颜色 (点击看大图)</em>

Let’s put them into a scheme to see **dependencies between modules**.

[![](https://rawgit.com/Bogdan-Lyashenko/Under-the-hood-ReactJS/7c2372e1/stack/images/intro/files-scheme.svg)](https://rawgit.com/Bogdan-Lyashenko/Under-the-hood-ReactJS/7c2372e1/stack/images/intro/files-scheme.svg)

<em>Intro.2 Modules dependencies (clickable)</em>

But, as you probably know, React is built to **support many environments**. Like mobile (**ReactNative**), browser (**ReactDOM**), also **Server Rendering** and **ReactART**(for drawing vector graphics using React) etc. So, a number of files actually is bigger than that. We can compare how actually multi-support affects the scheme.

[![](https://rawgit.com/Bogdan-Lyashenko/Under-the-hood-ReactJS/7c2372e1/stack/images/intro/modules-per-platform-scheme.svg)](https://rawgit.com/Bogdan-Lyashenko/Under-the-hood-ReactJS/7c2372e1/stack/images/intro/modules-per-platform-scheme.svg)

<em>Intro.3 Platform dependencies (clickable)</em>

So, you can see which parts were changed, it means they have separate implementation per platform. Let’s take something simple, like ReactEventListener, obviously, its implementation will be different for different platforms! Technically, as you can imagine, these platform dependent modules should be somehow injected or connected to the current logic flow, and, in fact, there are many such injectors as well. We omit them to simplify the scheme, there is nothing special in terms of coding, just standard composition pattern.

Let’s learn the logic flow for **React DOM in a regular browser**. It’s the most used one, and it completely covers all React’s architecture ideas, so, fair enough!


### Code sample

What is the best way to learn the code of a framework or library? That's right, read and debug the code. Alright, we are gonna to debug **two processes**: **ReactDOM.render** and **component.setState**, which map on mount and update. Let’s check the code we can write for a start. What do we need? Probably several small components with simple renders, so it will be easier to debug.

```javascript
class ChildCmp extends React.Component {
    render() {
        return <div> {this.props.childMessage} </div>
    }
}

class ExampleApplication extends React.Component {
    constructor(props) {
        super(props);
        this.state = {message: 'no message'};
    }

    componentWillMount() {
        //...
    }

    componentDidMount() {
        /* setTimeout(()=> {
            this.setState({ message: 'timeout state message' });
        }, 1000); */
    }

    shouldComponentUpdate(nextProps, nextState, nextContext) {
        return true;
    }

    componentDidUpdate(prevProps, prevState, prevContext) {
        //...
    }

    componentWillReceiveProps(nextProps) {
        //...
    }

    componentWillUnmount() {
        //...
    }

    onClickHandler() {
        /* this.setState({ message: 'click state message' }); */
    }

    render() {
        return <div>
            <button onClick={this.onClickHandler.bind(this)}> set state button </button>
            <ChildCmp childMessage={this.state.message} />
            And some text as well!
        </div>
    }
}

ReactDOM.render(
    <ExampleApplication hello={‘world’} />,
    document.getElementById('container'),
    function() {}
);
```

So, we are ready to start, let’s move on to the first part of the scheme. One by one part we will go through all of it.

[To the next page: Part 0 >>](./Part-0.md)


[Home](../../README.md)
