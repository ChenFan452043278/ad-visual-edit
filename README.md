# ad-visual-edit

## Installation

### With npm

```bash
npm i ad-visual-edit
```

### Props
#### list
Type: `Array`<br>
Required: `true`<br>
Default: `null`

广告列表的数据

#### uploadUrl
Type: `String`<br>
Required: `true`<br>
Default: `null`

对应项目上传图片的地址

#### postModule
Type: `Function`<br>
Required: `true`<br>
Default: `null`

获取编辑完成的广告列表数据

```html
<ad-visual-edit ref="advisualedit" :list="list" :postUrl="url" @postModule="getJson"></ad-visual-edit>
```

```javascript
created() {
  let data = {
    id: this.$route.query.id
  };
  screenBindGet(data).then(res => {
    this.list = res.d;
    this.$refs.advisualedit.showList(this.list);
  })
},
methods: {
  getJson(data) {
    let saveData = {
      id: this.$route.query.id,
      data: data
    }
    screenBindSave(saveData).then(res => {
      console.log(res);
      this.$message({
        type: 'success',
        message: res.m
      });
      this.$router.push({ path: '/ad-module/screen-config' });
    })
  }
}
```
