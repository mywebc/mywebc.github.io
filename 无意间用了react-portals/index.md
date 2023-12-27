# 无意间用了React Portals

> 不知哪天随手点开了一篇react portals的文章,眼前一亮,竟然还没有我听说过的react的特性,在看了大概之后,并没有觉得有什么用,不过最近在做开发时,还正好和需求对上了.

<!-- more -->
## 什么是react portal
* portal(入口)能够允许我们将自己的组件挂到一个另一个时空的指定节点去,这个节点跟我们当前的DOM树没有半毛钱关系;
```js
ReactDOM.createPortal(child, container)
```
* 第一个参数即为自定义组件,第二个参数即为指定节点

## portal出现的意义

* 如果在原有DOM树插入组件,影响DOM结构;
* 样式污染,组件所需数据流向混乱;

## 示例
```js
export default class YourComponent extends Component<YourComponentProps,YourComponentState> {
  private portalNode: HTMLDivElement;
  constructor(props: YourComponentProps) {
    super(props);
    const doc = window.document;
    this.portalNode = doc.createElement('div');
    doc.body.appendChild(this.portalNode);
  }
  componentWillUnmount() {
    window.document.body.removeChild(this.portalNode);
  }
  render() {
    return createPortal(
      <div>
        // your code...
      </div>,
      this.portalNode
    )
  }
}
```

## 支持事件冒泡
* 虽然说portal传送的节点与原本的DOM树没关系,但是这个组件还是支持事件冒泡的,在原本的父节点上同样能够监听到组件的事件;具体示例可以直接看官网;
[react portals](https://zh-hans.reactjs.org/docs/portals.html)


## 总结
* portal因为能跳出原来DOM节点,一般为用在弹框和提示框这种独立性强的组件上;




