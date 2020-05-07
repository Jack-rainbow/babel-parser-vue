# babel-parser-vue


`babel-parser-vue` 是一个自动将 `vue-js`代码转化为 `vue-ts`规范的工具库


## Install

```
npm install babel-parser-vue -g
```
全局安装完毕后，`npm`会自动将 `babel-parser-vue`执行文件的路径写入系统 `path`，所以理论上可以直接在命令行中使用 `babel-parser-vue`这个指令

## Usage

同时支持单文件和文件目录的转化

打开命令行工具，输入命令行指令，格式为：
```
babel-parser-vue vueFileFullPath
```

其中，`babel-parser-vue`是库的指令，第二个参数 `vueFileFullPath`表示需要处理的文件(夹)的 **完整全路径**

例如：

处理 `E:\project\testA\src\test.vue`文件，在命令行中输入：
```
babel-parser-vue E:\project\testA\src\test.vue
```
转化后的文件路径为
```
E:\project\testA\src\testTs.vue
```
处理 `E:\project\testA\src`文件夹下的所有 `.vue`文件，在命令行中输入：
```
babel-parser-vue E:\project\testA\src
```
转化后的文件夹路径为
```
E:\project\testA\srcTs
```
对于单文件来说，其必须是 `.vue`结尾，转化后的文件将输出到同级目录下，文件名为原文件名 + `Ts`，例如 `index.vue` => `indexTs.vue`

对于文件目录来说，程序将会对此文件目录进行递归遍历，找出这个文件夹下所有的 `.vue`文件进行转化，转化后的文件将按照原先的目录结构全部平移到同级目录下的一个新文件夹中，例如 `/src` => `/srcTs`

## Demo
```js
import OtherMixins from './OtherMixins'
export default {
  mixins: [OtherMixins],
  props: {
    a: {
      type: Number,
      required: true,
      validator (value) {
        return value > 2
      }
    }
  },
  data () {
    return {
      b: 20
    }
  },
  watch: {
    a (value) {
      console.log(value)
    }
  },
  computed: {
    c () {
      return this.a * 2
    }
  },
  created () {
    console.log('created done')
  },
  methods: {
    clickFn () {
      console.log('click')
    }
  }
}
```
转化为：
```js
import { Component, Mixins, Prop, Watch } from 'vue-property-decorator'
import OtherMixins from './OtherMixins'

@Component
export default class babel-parser-vue extends Mixins(OtherMixins) {
  @Prop({
    type: Number,
    required: true,

    validator(value) {
      return value > 2
    }

  })
  readonly a: number | undefined
  b: number = 20

  @Watch('a')
  onAChanged(value) {
    console.log(value)
  }

  get c() {
    return this.a * 2
  }

  created() {
    console.log('created done')
  }

  clickFn() {
    console.log('click')
  }

}
```