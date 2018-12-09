# 数据驱动
## new Vue发生了什么

1.`src/core/instance/index.js`
这边定义了vue的构造函数

``` javascript
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}
```

主要的函数在`_init`这个函数上，他定义在`initMixin`这个方法里。

2. `src/core/instance/init.js`


``` javascript
export function initMixin (Vue: Class<Component>) {
  Vue.prototype._init = function (options?: Object) {
    const vm: Component = this
    // a uid
    vm._uid = uid++

    let startTag, endTag
    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      startTag = `vue-perf-start:${vm._uid}`
      endTag = `vue-perf-end:${vm._uid}`
      mark(startTag)
    }

    // a flag to avoid this being observed
    vm._isVue = true
    // merge options
    if (options && options._isComponent) {
      // optimize internal component instantiation
      // since dynamic options merging is pretty slow, and none of the
      // internal component options needs special treatment.
      initInternalComponent(vm, options)
    } else {
      vm.$options = mergeOptions(
        resolveConstructorOptions(vm.constructor),
        options || {},
        vm
      )
    }
    /* istanbul ignore else */
    if (process.env.NODE_ENV !== 'production') {
      initProxy(vm)
    } else {
      vm._renderProxy = vm
    }
    // expose real self
    vm._self = vm
    initLifecycle(vm)
    initEvents(vm)
    initRender(vm)
    callHook(vm, 'beforeCreate')
    initInjections(vm) // resolve injections before data/props
    initState(vm)
    initProvide(vm) // resolve provide after data/props
    callHook(vm, 'created')

    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      vm._name = formatComponentName(vm, false)
      mark(endTag)
      measure(`vue ${vm._name} init`, startTag, endTag)
    }

    if (vm.$options.el) {
      vm.$mount(vm.$options.el)
    }
  }
}
```
这里主要分析`initstate`方法

3. `src/core/instance/state.js`

``` javascript
export function initState (vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props)
  if (opts.methods) initMethods(vm, opts.methods)
  if (opts.data) {
    initData(vm)
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  if (opts.computed) initComputed(vm, opts.computed)
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }
}
```
主要在于initData方法

4.`src/core/instance/state.js`

这个方法把data等都绑定在了vue的._data这个属性上面。
``` javascript
function initData (vm: Component) {
  let data = vm.$options.data
  data = vm._data = typeof data === 'function'
    ? getData(data, vm)
    : data || {}
  if (!isPlainObject(data)) {
    data = {}
    process.env.NODE_ENV !== 'production' && warn(
      'data functions should return an object:\n' +
      'https://vuejs.org/v2/guide/components.html#data-Must-Be-a-Function',
      vm
    )
  }
  // proxy data on instance
  const keys = Object.keys(data)
  const props = vm.$options.props
  const methods = vm.$options.methods
  let i = keys.length
  while (i--) {
    const key = keys[i]
    if (process.env.NODE_ENV !== 'production') {
      if (methods && hasOwn(methods, key)) {
        warn(
          `Method "${key}" has already been defined as a data property.`,
          vm
        )
      }
    }
    if (props && hasOwn(props, key)) {
      process.env.NODE_ENV !== 'production' && warn(
        `The data property "${key}" is already declared as a prop. ` +
        `Use prop default value instead.`,
        vm
      )
    } else if (!isReserved(key)) {
      proxy(vm, `_data`, key)
    }
  }
  // observe data
  observe(data, true /* asRootData */)
}
```
在看一下`proxy`这个方法


``` javascript
export function proxy (target: Object, sourceKey: string, key: string) {
  sharedPropertyDefinition.get = function proxyGetter () {
    return this[sourceKey][key]
  }
  sharedPropertyDefinition.set = function proxySetter (val) {
    this[sourceKey][key] = val
  }
  Object.defineProperty(target, key, sharedPropertyDefinition)
}
```

非常清晰，是定义了get和set属性，然后通过`Object.defineProperty`这个定义

## Vue 实例挂载实例的实现

在线编译版本(compile版本)的，先缓存原先的$mount方法，然后再生成一个$mount，进行template编译,转化为render函数.

```javascript
const mount = Vue.prototype.$mount
Vue.prototype.$mount = function (
  el?: string | Element,
  hydrating?: boolean
): Component {


...

const { render, staticRenderFns } = compileToFunctions(template, {
        shouldDecodeNewlines,
        shouldDecodeNewlinesForHref,
        delimiters: options.delimiters,
        comments: options.comments
      }, this)
      
```

然后再调用原来的$mount方法。
```javascript
return mount.call(this, el, hydrating)
```
最终会执行一个updateComponent方法，即一个渲染watcher，即每次更新数据的时候，都会重新渲染

## render函数
首先可以简单尝试下手写render函数

``` javascript
new Vue({
  el: '#app',
  render( createElement ) {
    return createElement('div', {
      attrs: {
        id: 'demo'
      }
    }, this.message)
  },
  data() {
    return {
      message: 'hello vue!woo'
    }
  }
})
```

vue的_render方法是实例的一个私有方法，他用来把实例渲染成一个虚拟node，定义在`src/core/instance/render.js`方法中

```
Vue.prototype._render = function (): VNode {
    const vm: Component = this
    const { render, _parentVnode } = vm.$options

    // reset _rendered flag on slots for duplicate slot check
    if (process.env.NODE_ENV !== 'production') {
      for (const key in vm.$slots) {
        // $flow-disable-line
        vm.$slots[key]._rendered = false
      }
    }

    if (_parentVnode) {
      vm.$scopedSlots = _parentVnode.data.scopedSlots || emptyObject
    }

    // set parent vnode. this allows render functions to have access
    // to the data on the placeholder node.
    vm.$vnode = _parentVnode
    // render self
    let vnode
    try {
      vnode = render.call(vm._renderProxy, vm.$createElement)
    } catch (e) {
      handleError(e, vm, `render`)
      // return error render result,
      // or previous vnode to prevent render error causing blank component
      /* istanbul ignore else */
      if (process.env.NODE_ENV !== 'production') {
        if (vm.$options.renderError) {
          try {
            vnode = vm.$options.renderError.call(vm._renderProxy, vm.$createElement, e)
          } catch (e) {
            handleError(e, vm, `renderError`)
            vnode = vm._vnode
          }
        } else {
          vnode = vm._vnode
        }
      } else {
        vnode = vm._vnode
      }
    }
    // return empty vnode in case the render function errored out
    if (!(vnode instanceof VNode)) {
      if (process.env.NODE_ENV !== 'production' && Array.isArray(vnode)) {
        warn(
          'Multiple root nodes returned from render function. Render function ' +
          'should return a single root node.',
          vm
        )
      }
      vnode = createEmptyVNode()
    }
    // set parent
    vnode.parent = _parentVnode
    return vnode
  }
```

主要方法是`vnode = render.call(vm._renderProxy, vm.$createElement)`，其中：
* _renderProxy定义在`proxy.js`中
* $createElement定义在`vdom/create-element`中

## virtual Dom

首先可以先把一个dom对象打印出来看看。

``` javascript
var div = document.createElement('div')
undefined
var str = ''
undefined
for(key in div){str+=key+' '}
"align title lang translate dir dataset hidden tabIndex accessKey draggable spellcheck autocapitalize contentEditable isContentEditable inputMode offsetParent offsetTop offsetLeft offsetWidth offsetHeight style innerText outerText onabort onblur oncancel oncanplay oncanplaythrough onchange onclick onclose oncontextmenu oncuechange ondblclick ondrag ondragend ondragenter ondragleave ondragover ondragstart ondrop ondurationchange onemptied onended onerror onfocus oninput oninvalid onkeydown onkeypress onkeyup onload onloadeddata onloadedmetadata onloadstart onmousedown onmouseenter onmouseleave onmousemove onmouseout onmouseover onmouseup onmousewheel onpause onplay onplaying onprogress onratechange onreset onresize onscroll onseeked onseeking onselect onstalled onsubmit onsuspend ontimeupdate ontoggle onvolumechange onwaiting onwheel onauxclick ongotpointercapture onlostpointercapture onpointerdown onpointermove onpointerup onpointercancel onpointerover onpointerout onpointerenter onpointerleave nonce click focus blur namespaceURI prefix localName tagName id className classList slot attributes shadowRoot assignedSlot innerHTML outerHTML scrollTop scrollLeft scrollWidth scrollHeight clientTop clientLeft clientWidth clientHeight attributeStyleMap onbeforecopy onbeforecut onbeforepaste oncopy oncut onpaste onsearch onselectstart previousElementSibling nextElementSibling children firstElementChild lastElementChild childElementCount onwebkitfullscreenchange onwebkitfullscreenerror setPointerCapture releasePointerCapture hasPointerCapture hasAttributes getAttributeNames getAttribute getAttributeNS setAttribute setAttributeNS removeAttribute removeAttributeNS hasAttribute hasAttributeNS toggleAttribute getAttributeNode getAttributeNodeNS setAttributeNode setAttributeNodeNS removeAttributeNode closest matches webkitMatchesSelector attachShadow getElementsByTagName getElementsByTagNameNS getElementsByClassName insertAdjacentElement insertAdjacentText insertAdjacentHTML requestPointerLock getClientRects getBoundingClientRect scrollIntoView scrollIntoViewIfNeeded animate computedStyleMap before after replaceWith remove prepend append querySelector querySelectorAll webkitRequestFullScreen webkitRequestFullscreen scroll scrollTo scrollBy createShadowRoot getDestinationInsertionPoints ELEMENT_NODE ATTRIBUTE_NODE TEXT_NODE CDATA_SECTION_NODE ENTITY_REFERENCE_NODE ENTITY_NODE PROCESSING_INSTRUCTION_NODE COMMENT_NODE DOCUMENT_NODE DOCUMENT_TYPE_NODE DOCUMENT_FRAGMENT_NODE NOTATION_NODE DOCUMENT_POSITION_DISCONNECTED DOCUMENT_POSITION_PRECEDING DOCUMENT_POSITION_FOLLOWING DOCUMENT_POSITION_CONTAINS DOCUMENT_POSITION_CONTAINED_BY DOCUMENT_POSITION_IMPLEMENTATION_SPECIFIC nodeType nodeName baseURI isConnected ownerDocument parentNode parentElement childNodes firstChild lastChild previousSibling nextSibling nodeValue textContent hasChildNodes getRootNode normalize cloneNode isEqualNode isSameNode compareDocumentPosition contains lookupPrefix lookupNamespaceURI isDefaultNamespace insertBefore appendChild replaceChild removeChild addEventListener removeEventListener dispatchEvent "
```

因此需要用一个js对象去描述一个dom节点，开销要小。定义在`src/core/vdom/vnode.js`,主要借鉴的是开源的snabbdom。

Vnod是对真实dom的一种描述，无非是几个关键属性，标签名、数据、子节点等，用来映射到真实的dom渲染，不需要包括操作dom的方法，因此是非常轻量和简单的。

## createElement

最终会调用这个方法创建vnode，定义在`src/core/vdom/create-element.js`