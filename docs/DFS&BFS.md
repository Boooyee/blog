### 深度优先遍历（Depth First Search）

获取全部标签 `let node = document.querySelector('*')`
递归版本：

`
 function deepFirstSearch(node, nodeList = []) {

      if (node) {
        nodeList.push(node)
        let children = node.children
        for (let i = 0; i < children.length; i++) {
          nodeList.push(...deepFirstSearch(children[i]))
        }
      }
      return nodeList
    }`

非递归版本：

`
  function deepFirstSearch(node) {

      var nodes = [];
      if (node != null) {
        var stack = [];
        stack.push(node);
        while (stack.length != 0) {
          var item = stack.pop();
          nodes.push(item);
          var children = item.children;
          for (var i = children.length - 1; i >= 0; i--)
            stack.push(children[i]);
        }
      }
      return nodes;
    }

`

### 广度优先遍历（breadth-first traverse）

``` 

function breadFirstSearch(node) {

      let stack = [node], result = []
      while (stack.length) {
        let list = stack.shift()
        result.push(list)
        let children = list.children
        for (let i = 0; i < children.length; i++) {
          stack.push(children[i])
        }
      }
      return result
    }
    let result = breadFirstSearch(node)

```
