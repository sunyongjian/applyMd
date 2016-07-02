# Vue和angular的一些区别
- 1  angular中的编译用 ng-bind vue中用v-text 都等同于{{}}
- 2  angular中的遍历是ng-repeat  保存$index 用ng-init保存
    vue和angular 都有数组(index,cur) in arr
    和对象 (key, value) in obj
    vue 是用in-for  angular是用 ng-repeat

# 关于Vue的组件
## prop

- 每个组件实例的作用域都是孤立的。这意味着不能在子组件的模板内直接引用。这时候我们就需要在实例化组件的时候，给他添加一个props属性
  prop是组件数据一个字段,期望从父组件 或者是Vue的实例即ViewModel 上传下来，子组件就需要显式的声明props :['message'] 
  这只是第一步，说明我已经准备好接受了。
- 字面量props

    还需要在自定义组件的标签里加上 一个 message="我需要的字符串" ,这样这段字符串就传到子组件里了

- 动态props

    如果是实例data里的变量，亦或是v-model绑定的(Vue通常会建议把这个不存在的变量放到实例的data中,否则就报错提醒，但是不影响使用)。就需要 一级一级的传给需要的组件。
  自定义标签内写  v-bind:message="变量"  简写为 :message="变量"

## 注意事项

- 父组件 <parent></parent> 里面不能直接嵌套 子组件的标签，因为
  这个自定义标签里面的内容解析的时候会被 template模板给替换掉。所以应该把
  子组件的自定义标签写在父组件的模板里面。才可以解析

- 由于Vue的模板使用的DOM模板，使用的浏览器的解析器
  ，而不是自己实现的。相比字符串模板，DOM模板有一些好处
  但是也有一些问题。必须是有效的html片段
  一些html元素里放什么标签是有限制的。
 1. 比如a不能包含其他的交互元素，比如按钮链接。
 2. ul ol 只能包括li 。。当然写别的可以解析。只是不符合规范
 3. select 只能包含 option 和 optgroup
 4. table 只能直接包含 thead, tbody, tfoot, tr, caption, col, colgroup
 5. tr 只能直接包含 th 和 td
    这就导致了一些意外情况的发生

- HTML 特性不区分大小写。名字形式为 camelCase 驼峰命名 的 prop 用作特性时，需要转为kebab-case（短横线隔开）

```
props:['myMessage']  ==> <child my-message="hello"></child>
```

- 当传递数字的时候，就不要用字面量的语法了 因为传递是字符串 '1',而不是实际的数字。 所以要用动态props传递，从而让它的值被当做javascript表达式计算
  只需要用 v-bind


```
-->props:['some-prop']
--><child some-prop="1"> 或者<child : some-prop="1">
--> {{some-prop+1}} ==> 11   {{some-prop+1}} ==> 2
```
