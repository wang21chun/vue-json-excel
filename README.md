# JSON to Excel for VUE 2
vue2.0^ Json数据导出Excel文件 基于<a href="https://github.com/SheetJS/js-xlsx">js-xlxs</a>

## ✔ Getting started

Get the package:
```bash
npm install vue-json-excel --save
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

In your HTML call it like

```html
<download-excel
	class   = "btn btn-default"
	:data   = "json_data"
	:fields = "json_fields"
	name    = "filename">

	Download Excel (you can customize this with html code!)

</download-excel>
```
json_data contains the data you want to export, json_fields is a type mapping
for the fields in the json, name is the file name.

## License
MIT



#### Status
This project is in an early stage of development. Any contribution is welcome :D
