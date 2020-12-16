# MutationObserver

是一个living standard了，可以detect很多种change

> [`subtree`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/subtree) OptionalSet to `true` to extend monitoring to the entire subtree of nodes rooted at `target`. All of the other `MutationObserverInit` properties are then extended to all of the nodes in the subtree instead of applying solely to the `target` node. The default value is `false`.
>
> [`childList`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/childList) OptionalSet to `true` to monitor the target node \(and, if `subtree` is `true`, its descendants\) for the addition of new child nodes or removal of existing child nodes. The default value is `false`.
>
> [`attributes`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/attributes) OptionalSet to `true` to watch for changes to the value of attributes on the node or nodes being monitored. The default value is `true` if either of `attributeFilter` or `attributeOldValue` is specified, otherwise the default value is `false`.
>
> [`attributeFilter`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/attributeFilter) OptionalAn array of specific attribute names to be monitored. If this property isn't included, changes to all attributes cause mutation notifications.
>
> [`attributeOldValue`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/attributeOldValue) OptionalSet to `true` to record the previous value of any attribute that changes when monitoring the node or nodes for attribute changes; see [Monitoring attribute values](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver#Monitoring_attribute_values) in [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) for details on watching for attribute changes and value recording. The default value is `false`.
>
> [`characterData`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/characterData) OptionalSet to `true` to monitor the specified target node \(and, if `subtree` is `true`, its descendants\) for changes to the character data contained within the node or nodes. The default value is `true` if `characterDataOldValue` is specified, otherwise the default value is `false`.
>
> [`characterDataOldValue`](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserverInit/characterDataOldValue) OptionalSet to `true` to record the previous value of a node's text whenever the text changes on nodes being monitored. For details and an example, see [Monitoring text content changes](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver#Monitoring_text_content_changes) in [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver). The default value is `false`.

如果不在attributes上面设置style的话，能detect resize吗？

好像可以听resize（珏宇我错了- -）：

{% code title="https://codepen.io/webgeeker/pen/YjrZgg" %}
```javascript
    const MutationObserver = window.MutationObserver || window.WebKitMutationObserver || window.MozMutationObserver;
    // target
    const list = document.querySelector(`ol`);
    // options
    const config = {
        attributes: true,
        childList: true,
        characterData: true,
        subtree: true,
    };
    // instance
    const observer = new MutationObserver(function(mutations) {
        mutations.forEach(function(mutation) {
            if (mutation.type === "attributes") {
                console.log("mutation =", mutation);
                console.log(`The \`${mutation.attributeName}\` attribute was modified.`);
                let {
                    width,
                    height,
                } = list.style;
                let style = {
                    width,
                    height
                };
                console.log("style =\n", JSON.stringify(style, null, 4));
            }
        });
    });
    observer.observe(list, config);
```
{% endcode %}

