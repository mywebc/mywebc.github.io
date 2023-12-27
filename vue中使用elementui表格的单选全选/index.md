# vue中使用elementUI，表格的单选全选


在vue中使用elementUI中时表格，如何拿到单选或者多选（全选）的值？先记下一笔。 
<!--more-->
```javascript
// 监听单选 val表示当前选中行， row表示所有的表格数据 
selectChange(val, row) { 
	let flag = 0 for(let i in val) { 
		if(row.volumeNo === val.volumeNo) { 
			// 如果已勾选，push进存值数组 flag = 1 break 
		} 
	} 
	if (flag === 1) { 
		this.codeList.push(row) 
	} else { 
		// 如果取消勾选，找到存值的数组并且删除 
		for(let i in this.codeList) { 
			if(this.codeList.volumeNo === row.volumeNo) {
				 this.codeList.splice(i, 1) 
				} 
			} 
		} 
	} 
	// 监听全选, val表示所有数据， volumeNoData是在计算属性中请求的所有数据 
	selectAll (val) { 
		var v = this if (val.length === 0) { 
			for( let i in v.volumeNoData) { 
				// 如果取消全选 
				for (let j in v.codeList) { 
					if (v.codeList.volumeNo === v.volumeNoData.volumeNo) { 
						v.codeList.splice(j, 1) 
					} 
				} 
			} 
		} 
		if (v.codeList.length === 0) { 
		//如果点了全选，而存值数组里为空的话，全部push 
		for (let i in val) {
			 v.codeList.push(val) 
			} 
		} else {
		 // 否则一一对比，push没有的 
		 for (let i in val) { 
		 	let flag = false for(let j in v.codeList) { 
		 	if(v.codeList.volumeNo === val.volumeNo) {
		 	 flag = true break 
		 	} 
		 } if(!flag) { 
		 	v.codeList.push(val) 
		 	} 
	 	} 
 	} 
 } 
```
