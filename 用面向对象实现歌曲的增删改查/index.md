# 用面向对象实现歌曲的增删改查


> 自定义构造函数，在其原型对象上书写增删改查函数，再调用拼接字符串到HTML页面即可； 构造函数如下：

<!--more-->

```javascript
function Mp3 (songs){//传入歌曲
    this.songList=songs||\[\];//如果传入了歌曲就让歌曲数组等于他，否则等于空数组
    // 要初始化界面
    this.init();
    	}
    /*在它的原型对象中书写增删改查方法*/
    Mp3.prototype={
    // 已经替换了原型对象用constructor会丢失，需要手动设置
    constructor:Mp3,
    /*书写初始化函数，进来时如果有数组，就要把他们拼接好并且全部显示到界面上*/
    init:function(){
    // render就是渲染的意思，我们要为他写函数
    this.render();
    	},
    render:function(){
    // 把歌曲信息拼接好塞到父盒子里
    var listDiv=document.querySelector('#c');
    // 定义一个空数组用来装拼接的字符串
    var strArr=\[\];
    for (var i = 0; i < this.songList.length; i++) {
    var song=this.songList\[i\];
    //这里用push往数组里加这样一个字符串
    strArr.push('

'+
		'

'+song.name+'

'+
		'

'+song.singer+'

'+
		'

');
    		}
    //join() 方法用于把数组中的所有元素放入一个字符串,如果省略参数以逗号为分隔符
    var str=strArr.join('');
    listDiv.innerHTML=str;
    	},
    //增加
    addSong:function(songName,singer){
    if (!songName||!singer){
    	throw'内容不能为空！';
    		};
    var temp={name:songName,singer:singer};
    this.songList.push(temp);
    this.render();
    return temp;
    	},
    // 删除
    removeSong:function(songName){
        var song=this.selectSong(songName);
        var index=this.songList.indexOf(song);
        // 判断有没有找到
        if(index!=-1){
        this.songList.splice(index,1);
        this.render();
            return true;
       }else {
    	    return false;
    	}
    },
    // 改
    updateSong:function(songName,singer){
    var song=this.selectSong(songName);
    if(song==null){
    	return null;
    	}else {
    		song.singer=singer;
    		this.render();
    		return song;
    		}

    	},
    // 查
    selectSong:function(songName){
    for (var i = 0; i < this.songList.length; i++) {
    	var song=this.songList\[i\];
    	if(song.name==songName){
    		return song;
    			}
    		}
    	return null;
    	}
    };
```

