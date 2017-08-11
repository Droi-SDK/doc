## webpack loaders

### vue-loader
处理代码

### weex-vue-loader
1. parse
1)cache处理，有cache优先使用cache
2)`vue-template-compile`的`parseComponent`->将一个`.vue`文件转成一个`JSONObject`（过程主要是对html tag进行解析），如下所示：
``` js
{ template:
   { type: 'template',
     content: '\n<div class="wrapper" @click="update">\n  <image :src="logoUrl" class="logo"></image>\n  <text class="title"  style="color:red">Hello {{target}}</text>\n</div>\n',
     start: 10,
     attrs: {},
     end: 175 },
  script:
   { type: 'script',
     content: '//\n//\n//\n//\n//\n//\n//\n//\n//\n//\n//\n//\n//\n\nexport default {\n  data: {\n    logoUrl: \'https://alibaba.github.io/weex/img/weex_logo_blue@3x.png\',\n    target: \'World\',\n    time: 0\n  },\n  methods: {\n    update: function (e) {\n      this.time++\n      this.target = \'Weex\'\n      console.log(\'target:\', this.target, this.time)\n    }\n  }\n}\n',
     start: 339,
     attrs: {},
     end: 656 },
  styles:
   [ { type: 'style',
       content: '\n\n\n\n\n\n\n\n.wrapper { align-items: center; margin-top: 120px; }\n.title { font-size: 48px; }\n.logo { width: 360px; height: 82px; }\n',
       start: 195,
       attrs: {},
       end: 321 } ],
  customBlocks: [] }
```

3)生成如下内容
``` js
var __vue_exports__, __vue_options__
var __vue_styles__ = []

/* styles */
__vue_styles__.push(require("!!weex-vue-loader/lib/style-loader!weex-vue-loader/lib/style-rewriter?id=data-v-6c7c4094!weex-vue-loader/lib/selector?type=styles&index=0!./foo.vue")
)

/* script */
__vue_exports__ = require("!!weex-vue-loader/lib/script-loader!babel-loader!weex-vue-loader/lib/selector?type=script&index=0!./foo.vue")

/* template */
var __vue_template__ = require("!!weex-vue-loader/lib/template-compiler?id=data-v-6c7c4094!weex-vue-loader/lib/selector?type=template&index=0!./foo.vue")
__vue_options__ = __vue_exports__ = __vue_exports__ || {}
if (
  typeof __vue_exports__.default === "object" ||
  typeof __vue_exports__.default === "function"
) {
if (Object.keys(__vue_exports__).some(function (key) { return key !== "default" && key !== "__esModule" })) {console.error("named exports are not supported in *.vue files.")}
__vue_options__ = __vue_exports__ = __vue_exports__.default
}
if (typeof __vue_options__ === "function") {
  __vue_options__ = __vue_options__.options
}
__vue_options__.__file = "C:\\develope\\weex-1\\src\\foo.vue"
__vue_options__.render = __vue_template__.render
__vue_options__.staticRenderFns = __vue_template__.staticRenderFns
__vue_options__._scopeId = "data-v-6c7c4094"
__vue_options__.style = __vue_options__.style || {}
__vue_styles__.forEach(function (module) {
  for (var name in module) {
    __vue_options__.style[name] = module[name]
  }
})
if (typeof __register_static_styles__ === "function") {
  __register_static_styles__(__vue_options__._scopeId, __vue_styles__)
}

module.exports = __vue_exports__
```

2. styles,script,template单独再做处理
1)style -> `selector`/`import src` -> `style-rewriter`->`style-loader`
``` js
require("!!weex-vue-loader/lib/style-loader!weex-vue-loader/lib/style-rewriter?id=data-v-6c7c4094!weex-vue-loader/lib/selector?type=styles&index=0!./foo.vue")
```

2)script -> `selector`/`import src` -> `babel-loader` -> `script-loader`
``` js
require("!!weex-vue-loader/lib/script-loader!babel-loader!weex-vue-loader/lib/selector?type=script&index=0!./foo.vue")
```

3)template -> `selector`/`import src` -> `template-loader`(可选) -> `template-compiler` -> `weex-template-compiler` -> `vue-template-es2015-compiler`
``` js
require("!!weex-vue-loader/lib/template-compiler?id=data-v-6c7c4094!weex-vue-loader/lib/selector?type=template&index=0!./foo.vue")
```

3. webpack

## framework与运行时

### vue & weex-vue-render
作用与 native 中的`native代码`和`native framework js`相同

### weex-vue-framework（native js）
1. setup(frameworks)
1)register service
2)freezePrototype
3)init frameworks
4)set global methods
  - createInstance
  - destroyInstance
  - refreshInstance
  - getRoot `@deprecated`
  - receiveTasks(fireEvent,callback)
  - registerModules
  - registerComponents
2. 

