# Less的绝对路径引入

> 需求点：需要通过命令行建立通用文件，此时对于less文件来说，less文件中需要import一个通用的less变量库fn.less，此时需要通过绝对路径引入该通用fn.less

### 实现方式

通过webpack的配置`resolve.modules`来配置less的通用库fn.less

【注】该种需求使用`resolve.alias`不起作用，别名无法被less识别

配置如下，此处一定要写上`node_modules`，不然可能会造成其他的npm模块无法使用。

该配置表示告诉`webpack`解析模块时应该搜索的目录。该配置默认值是`['node_modules']`，所以在添加其他文件夹时千万要带上`node_modules`，且搜索目录顺序是按照配置顺序进行搜索，下方配置将优先搜索`src`文件夹。

```
resolve: {
  modules: [
    'src',
    'node_modules'
  ]
}
```

配置完成后，如果`src`文件夹下有`styles`文件夹，`styles`文件夹内含有`fn.less`文件，则在其他less文件中可以如下方式填写绝对引入。

```
@import '~styles/fn.less';
```

Over~~

