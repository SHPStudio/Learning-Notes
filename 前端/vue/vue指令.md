# 标签的vue指令
## v-bind:attribute=xxx
  这个可以把标签的属性值与vue对象中data域下的以xxx为key的值进行绑定。
  **他可以使用缩写** :attribute
## v-if=xxx
  可以用于根据xxx的true和false去决定该标签是否可以显示
## v-for="xx in xxx"
  这个可以绑定vue中一个数组对象去渲染一个列表
  这个xx代表了列表中的一个单体对象，如果每个单独的实例是`{ text: "xxx" }`那么xx就代表了这个对象，
  xxx是代表了vue中定义的列表对象。那么在定义好之后再在标签的内容中添加引用数据即可`{{ xx.text }}`
## v-on:click=method
  这个是在相应的标签上绑定相应的事件，事件的相应方法对应的是vue对象中methods下的自定义方法。
  **他也可以使用缩写** @click
## v-model:data
  这个添加到input标签上可以非常容易的实现双向绑定。
## v-once
  这个指令加到标签上代表这个标签所绑定的数据只会被应用一次。
## v-html="rawHtml"
  这个指令可以把`{{ msg }}`绑定的文本转换成html文本。一般经过这个插值绑定的都会转成纯文本的形式，所以如果需要
  添加html文本的时候可能会用到这个。不过并不建议这么做，因为可能遭到 XSS 攻击。