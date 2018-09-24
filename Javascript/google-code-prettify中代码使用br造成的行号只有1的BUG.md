在使用code-prettify的时候，有遇到一个BUG；

当时使用的是国内一个博客提供给的链接，下载来后，有问题的；


问题:当从webstrome内复制的代码，直接复制近WLW的时候，行号只有第一行；其它的没有了；

clone github的源码发现；

这个BUG已经被别人修复了；但是它的压缩包和src文件内还是以前的；

需要手动修改下；

找到 walk 这个方法；


```javascript
function walk(node) {
  var type = node.nodeType;
  if (type == 1 && !nocode.test(node.className)) {  // Element
      if ('br' === node.nodeName.toLowerCase()) {
          breakAfter(node);
      // Discard the <BR> since it is now flush against a </LI>.
      if (node.parentNode) {
        node.parentNode.removeChild(node);
      }
    } else {
      for (var child = node.firstChild; child; child = child.nextSibling) {
        walk(child);
      }
    }
  } else if ((type == 3 || type == 4) && isPreformatted) {  // Text
    var text = node.nodeValue;
    var match = text.match(lineBreak);
    if (match) {
      var firstLine = text.substring(0, match.index);
      node.nodeValue = firstLine;
      var tail = text.substring(match.index + match[0].length);
      if (tail) {
        var parent = node.parentNode;
        parent.insertBefore(
          document.createTextNode(tail), node.nextSibling);
      }
      breakAfter(node);
      if (!firstLine) {
        // Don't leave blank text nodes in the DOM.
        node.parentNode.removeChild(node);
      }
    }
  }
}

```

请把

```
– if (‘br’ === node.nodeName) {
//修改为下面这个
+ if (‘br’ === node.nodeName.toLowerCase()) {
```
然后压缩后就可以了
