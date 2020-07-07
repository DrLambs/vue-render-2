<template>
  <div class="render">
    <h1>{{ msg }}</h1>
    <renderComponent></renderComponent>
    <renderComponentList></renderComponentList>
    <renderButton />
    <h1>JSX</h1>
    <renderButtonWidthJSX />
  </div>
</template>

<script>
import BaseButton from './BaseButton'

export default {
  name: 'RenderTest',
  props: {
    msg: String,
    level: {
      type: Number,
      required: true
    },
    items: {
      type: Array,
      required: false
    }
  },
  components: {
    renderComponent: {
      /**
       * render: 渲染函数
       * 参数: createElement
       * 参数类型: Function
       */
      render: function(createElement) {
        let _this = this['$options'].parent
        console.log(_this)
        console.log(_this.$slots)
        console.log(_this.$scopedSlots.default(), _this.$scopedSlots.header())
        // let _header = _this.$slots.header
        let _header = _this.$scopedSlots.header()
        /**
         * createElement 本身也是一个函数，它有三个参数
         *
         * 返回值: VNode
         *
         * 1. 一个 HTML 标签字符串，组件选项对象，或者解析上述任何一种的一个 async 异步函数。必需参数。{String | Object | Function} - 就是你要渲染的最外层标签
         *
         * 2. 一个包含模板相关属性的数据对象你可以在 template 中使用这些特性。可选参数。{Object} - 1中的标签的属性
         *
         * 3. 子虚拟节点 (VNodes)，由 `createElement()` 构建而成，也可以使用字符串来生成“文本虚拟节点”。可选参数。{String | Array} - 1的子节点，可以用 createElement() 创建，文本节点直接写就可以
         */
        return createElement(
          'h' + _this.level, // 标签名称 - 第一个参数
          {
            style: {
              color: 'gray',
              border: '1px solid pink'
            },
            scopedSlots: {
              default: props => createElement('span', props.text)
            }
          }, // 第二个单数
          [
            // _this.$slots.default,
            _this.$scopedSlots.default(),
            'child',
            createElement(
              'div',
              {
                class: {
                  slotsWrap: true
                }
              },
              [createElement('div', _header)]
            )
          ] // 子元素数组 - 第三个参数
        )
      }
    },
    renderComponentList: {
      render: function(createElement) {
        let _this = this['$options'].parent
        if (_this.items.length) {
          return createElement(
            'ul',
            _this.items.map(function(item) {
              return createElement('li', item.name)
            })
          )
        } else {
          return createElement('p', 'No items found.')
        }
      }
    },
    renderButton: {
      render: function(createElement) {
        const _this = this['$options'].parent
        return createElement(BaseButton, {
          // 与 `v-bind:class` 的 API 相同，接受一个字符串、对象或字符串和对象组成的数组
          class: {
            'base-button': true,
            'ui-button': false
          },
          // 与 `v-bind:style` 的 API 相同，接受一个字符串、对象，或对象组成的数组
          style: {
            // color: 'red',
            fontSize: '14px'
          },
          // 普通的 HTML attribute，会加到 BaseButton 最外层的元素上
          attrs: {
            id: 'base-button'
          },
          // 组件 prop，就是我们正常使用子组件时的那些 props
          props: {
            type: 'warning',
            size: 'large',
            contentObj: {
              text: 'Confirm',
              icon: '❓'
            }
          },
          // DOM property，以子组件最外层元素为父元素，将其内容替换为 `innerHTML` 中的内容
          // domProps: {
          //   innerHTML: 'button 没了，变成这段文字'
          // },
          // 事件监听器在 `on` 内，但不再支持如 `v-on:keyup.enter` 这样的修饰器。需要在处理函数中手动检查 keyCode。
          // 子组件 `$emit` 的事件才能接收到，否则在下一个属性才能监听到
          on: {
            click: _this.clickHandler,
            dblclick: _this.dblclickHandler
          },
          // 仅用于组件，用于监听原生事件，而不是组件内部使用 `vm.$emit` 触发的事件。
          nativeOn: {
            dblclick: _this.nativeClickHandler
          },
          // 自定义指令。略
          // directives: [],
          // 作用域插槽的格式为：{ name: props => VNode | Array<VNode> }
          scopedSlots: {
            default: props => {
              console.log(111, props)
              if (props.children) {
                return createElement('span', props.children.text + ' ?')
              }
              return createElement('span', 'defult')
            }
          },
          // 如果组件是其它组件的子组件，需为插槽指定名称。这个属性我也没明白
          slot: 'title'
          // 其它特殊顶层 property。略
          // key: '',
          // ref: '',
          // refInFor: true
        })
      }
    },
    renderButtonWidthJSX: {
      render(h) {
        const _this = this['$options'].parent
        const contentObj = { text: 'jsx' }

        return (
          <BaseButton
            type="danger"
            size="small"
            contentObj={contentObj}
            style={{ fontSize: '12px' }}
            class="jsx-button"
            onclick={_this.clickHandler}
            scopedSlots={{
              title: () => <h1>jsx title</h1>,
              // button: () => 'Delete',
              default: () => <span>default</span>
            }}
          />
        )

        // return (
        //   <BaseButton
        //     {...{
        //       props: {
        //         type: 'danger',
        //         size: 'small',
        //         contentObj,
        //         style: { fontSize: '12px' },
        //         class: 'jsx-button'
        //       },
        //       on: {
        //         click: _this.clickHandler,
        //         dblclick: _this.dblclickHandler
        //       },
        //       nativeOn: {
        //         dblclick: _this.nativeClickHandler
        //       },
        //       scopedSlots: {
        //         title: () => <h1>jsx title</h1>,
        //         // button: () => 'Delete',
        //         default: () => <span>default</span>
        //       }
        //     }}
        //   />
        // )
      }
    }
  },
  methods: {
    clickHandler(data) {
      console.log('clickHandler!', data)
    },
    dblclickHandler(data) {
      console.log('dblclickHandler!', data)
    },
    nativeClickHandler(data) {
      console.log('nativeClickHandler!', data)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

