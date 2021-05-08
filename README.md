# hidoc-editor
## 介绍
基于Tinymce5.7.1实现的富文本编辑器
## 安装

**请先下载Node.js**
1.下载npm包:

VUE项目的根目录下运行：
```
npm install hidoc-editor
```
2.在node_modules中找到hidoc-editor，将dist下的lib文件夹拷贝到public中

在index.html中,引用lib文件夹下的tinymce.min.js

```
<script src="lib/tinymce.min.js"> </script>
```

3.引入本npm包并注册为vue的组件，两种方式选一种即可：

（方式1）在你的vue项目的入口js中注册为全局的vue组件，然后在所有vue文件中都可以直接使用:
>例如:在main.js中

```
import Vue from 'vue'
import Editor from 'hidoc-editor';
Vue.component('Editor', Editor);

```
>然后在需要使用的vue页面中

```
<template>
    <div>
        <Editor v-model='content' @save="save"></Editor>
    </div>
</template>
```


（方式2）在需要使用的vue页面中单独引入并注册为一个局部的组件:
>例如:demo.vue：

```
<template>
    <div>
        <Editor v-model='content' @save="save"></Editor>
	</div>
</template>

<script>
import Editor from 'hidoc-editor';//引入本组件

export default {
  components: {
    Editor
  },
  data() {
    return {
      content: "在此输入···", //编辑器文本
    };
  },
  methods: {
    //保存编辑器的内容
    save() {
      console.log("保存编辑器内容");
      console.log(this.content);
    },
  },
};

</script>
```
## 使用

### 事件

| 事件名称 |            说明            |    回调函数    |
| :------: | :------------------------: | :------------: |
|   save   | 点击编辑器保存按钮时的回调 | (event: Event) |

### 属性

|   参数   |                        说明                        |  类型  |
| :------: | :------------------------------------------------: | :----: |
| content  |         编辑器的文本，格式为html。默认为空         | String |
|  height  |              编辑器的高度，默认为600               | Number |
| plugins  |            编辑器用到的插件，具体见下表            | String |
| toolbar  |             工具栏，使用需添加对应插件             | String |
| menubar  | 菜单栏，默认为"file edit insert view format table" | String |
| readonly |   编辑器是否只读，1为只读，其他为可编辑，默认为0   | Number |

#### menubar

菜单栏显示的功能，默认为"file edit insert view format table"

按传入字符串顺序显示，传入‘false’代表隐藏菜单栏

例：

```menubar: "file view format table"```

隐藏菜单栏：

```menubar: "false"```



#### toolbar

工具栏显示的功能

按顺序显示，可用|分割，传入‘false’代表隐藏工具栏

例：

`toolbar: "undo redo | bold italic| ltr rtl |forecolor removeformat| importword save"`

隐藏工具栏：

`toolbar: "false"`



#### plugins

print 打印编辑器内容 ctrl+p

preview 预览编辑器内容

paste 粘贴 ctrl+v

image 图片操作

importcss 插入代码

searchreplace 查找替换 ctrl +f

autolink 自动链接

autosave 自动保存

save 保存 ctrl + s

directionality 文字方向 在toolbar中有'ltr'`,'rtl'分别对应左右对齐

code 源码编辑，可查看html源代码

visualblocks 显示出块级元素

visualchars 显示出隐藏字符

fullscreen 全屏显示

link 插入链接 

media 插入媒体

template 插入模板

codesample 代码段

table 表格操作

hr 水平分割线

pagebreak 分页符

nonbreaking 不间断空格

anchor 锚点

toc 生成标题,根据页面中的h1,h2等标签生成

insertdatetime 插入时间

advlist 项目编号

lists 列表

wordcount 字数统计

imagetools 图片工具

textpattern 用于类markdown格式

noneditable 不可编辑，元素的class="mceNonEditable"则不可编辑

help 帮助

charmap 插入特殊符号

quickbars 快捷菜单

emoticons 表情，需数据库字符集支持utf8mb4

importword 从word导入，仅支持docx格式

indent2em 首行缩进

#### 案例

```
<template>
    <div>
        <Editor
        v-model="content"
        :toolbar="toolbar"
        :plugins="plugins"
        :height="height"
        :menubar="menubar"
        @save="saveEd"
        />
    </div>
</template>

<script>
import Editor from 'hidoc-editor';//引入本组件

export default {
  components: {
    Editor
  },
  data() {
    return {
      content: "",
      toolbar:
        "undo redo | bold italic| ltr rtl |forecolor removeformat| importword save",
      plugins:
      "importword save",
      menubar: "file edit insert view format",
      height: 500,
    };
  },
  methods: {
    //保存编辑器的内容
    save() {
      console.log("保存编辑器内容");
      console.log(this.content);
    },
  },
};

</script>
```


## 注意

- 编辑器支持常用快捷键，如Ctrl+S保存，Ctrl+F查找替换，Ctr+Z撤销等
- 从word导入功能，仅支持docx格式
- 保存表情时若出现问题
  `Incorrect string value: '\xF0\x9F\x98\x82\xF0\x9F...' for column 'x' at row 1`
  是因为存储emoji表情需要数据库支持，可以尝试把数据库的字符集改为utf8mb4，参考：https://www.cnblogs.com/h--d/p/5712490.html
- 上传分辨率过大的图片可能会出现卡顿的现象，且数据库可能无法存储过大的图片，需要插入图片后将其分辨率缩小。

