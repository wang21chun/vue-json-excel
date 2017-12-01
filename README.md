# JSON to Excel for VUE 2
vue2.0^ Json数据导出Excel文件 基于<a href="https://github.com/SheetJS/js-xlsx">js-xlxs</a>
在<a href="https://github.com/jecovier/vue-json-excel">这位朋友</a>的基础上修改的



## ✔ Getting started

Get the package:
```bash
npm install vue-json-excel file-saverjs  --save
```

Register JsonExcel in your app entrypoint:
```js
import Vue from 'vue'
import JsonExcel from 'vue-json-excel';

Vue.component('downloadExcel', JsonExcel);

const app = new Vue({
	el: '#app',
	data: {
		json_fields : ['field1','field2','field3','field4','field5'],
		json_data : [['value1','value2','value3','value4','value5'],
					 ['value1','value2','value3','value4','value5']]
	}
```

html中的写法

```html
<download-excel
	:data   = "json_data"
	:fields = "json_fields"
	name    = "filename">

	Download Excel (you can customize this with html code!)

</download-excel>
```

## License
MIT
