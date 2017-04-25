# 通达td_sdk调用说明

1、td_sdk使用步骤
    
    1.0概述
    
        
        产品使用td_sdk的模块，同一套代码运行在四个客户端里，每个模块页面均包含 pda/header.php。
        
        
        头文件：header_dd.php,header_client.php,header_wx.php,header_common.php
        

        打包文件：
        
        
        
        /static/pack/mobile/js/client.js
        
        /static/pack/mobile/js/common.js
        
        /static/pack/mobile/js/wx.js
        
        /static/pack/mobile/js/dd.js
        
        
       打包文件与源文件的映射关系：
        
        
        
        "/static/pack/mobile/js/wx.js": [
        
        
            "/static/mobile/js/zepto.min.js",
            
            "/static/mobile/js/gmu/gmu.js",
            
            "/static/mobile/js/gmu/list.js",
            
            "/static/mobile/js/gmu/page.js",
            
            "/static/mobile/js/frozen/frozen.js",
            
            "/static/mobile/js/swiped/swiped.js",
            
            "/static/mobile/js/routie.min.js",
            
            "/static/mobile/js/sdk.js",
            
            "/pda/js/jweixin-1.0.0.js",
            
            "/static/mobile/js/sdk/wx_sdk.js"
       
       ]
       
       "/static/pack/mobile/js/dd.js": [
            
            "/static/mobile/js/zepto.min.js",
            
            "/static/mobile/js/gmu/gmu.js",
            
            "/static/mobile/js/gmu/list.js",
            
            "/static/mobile/js/gmu/page.js",
            
            "/static/mobile/js/frozen/frozen.js",
            
            "/static/mobile/js/swiped/swiped.js",
            
            "/static/mobile/js/routie.min.js",
            
            "/static/mobile/js/sdk.js",
            
            "/static/mobile/js/sdk/dd_sdk.js"
        
        ]
        
        "/static/pack/mobile/js/common.js": [
            
            "/static/mobile/js/zepto.min.js",
            
            "/static/mobile/js/gmu/gmu.js",
            
            "/static/mobile/js/gmu/list.js",
            
            "/static/mobile/js/gmu/page.js",
            
            "/static/mobile/js/frozen/frozen.js",
            
            "/static/mobile/js/swiped/swiped.js",
            
            "/static/mobile/js/routie.min.js",
            
            "/static/mobile/js/sdk.js",
            
            "/static/mobile/js/sdk/h5_sdk.js"
        
        ]
        
        "/static/pack/mobile/js/client.js": [
            
            "/static/mobile/js/zepto.min.js",
            
            "/static/mobile/js/gmu/gmu.js",
            
            "/static/mobile/js/gmu/list.js",
            
            "/static/mobile/js/gmu/page.js",
            
            "/static/mobile/js/frozen/frozen.js",
            
            "/static/mobile/js/swiped/swiped.js",
            
            "/static/mobile/js/routie.min.js",
            
            "/static/mobile/js/sdk.js",
            
            "/static/mobile/js/sdk/client_sdk.js"
        
        ]
        
        
        页面在不同平台下，引入对应的js文件，最终均提供一个全局对象tMobileSDK，对象tMobileSDK负责统一不同平台提供的功能接口，如上传图片，选人等等。例如选择附件功能，钉钉提供方法dd.selectFile，客户端提供方法td.selectFile，经tMobileSDK封装后，在业务代码中，研发人员仅需要使用tMobileSDK.selectFile即可，无须考虑代码在哪个客户端运行。
        
       
    1.1引入js文件
        
        以客户端为例：
        
        /static/mobile/js/td.js
        /static/pack/mobile/js/client.js
        
        
    
    1.2通过ready处理成功验证
        
        页面加载时调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行
        tMobileSDK.ready = function(func){
            td.ready(func)
        };
        
    1.3接口调用说明
        
        onSuccess接口调用成功时执行的回调函数
        onFail接口调用失败时执行的回调函数

2、暴露的全局对象 tMobileSDK 
```

tMobileSDK = window.tMobileSDK || {};

```

3、确认弹窗
```
tMobileSDK.confirm = function(opts) {

    td.confirm({

        title: opts.title || '提示',//弹窗标题

        message: opts.message || '',//弹窗提示内容

        buttonLabels: opts.buttonLabels || ['确定','取消'],//弹窗底部确定和取消按钮文字

        onSuccess: opts.onSuccess,//点击确定回调函数，返回值result是点击按钮的索引值

        onFail: opts.onFail//点击取消回调函数，返回值err是错误信息

    })

}
```
4、顶部提示条
```
tMobileSDK.toast = function(opts) {

    td.toast({

        text: opts.text || '',//提示文字，默认值是"发送成功"

        duration: opts.duration || 5,//持续时间s

        delay: opts.delay || 0,//延时s

        onSuccess: opts.onSuccess,

        onFail: opts.onFail//返回值err是错误信息

    })

}
```
5、定位
```
/*
onSuccess获取成功回调，返回值result是位置信息
形如{ latitude: '11232423.423', longitude: '324424234.423423', addstr: '紫竹院路兵器大厦' }
*/
tMobileSDK.getLocation = function(opts) {

    td.getLocation({

        onSuccess: opts.onSuccess,

        onFail: opts.onFail//返回值err是错误信息

    })

}
```

6、开启其它的应用

```
tMobileSDK.launchApp = function(opts) {

    td.launchApp({

        app: opts.app || '',//appid

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
7、选人
```
tMobileSDK.selectUser = function(opts) {

    td.selectUser({

        onSuccess: opts.onSuccess,//返回值result是所选的用户信息： [ { uid:0, userName:'系统管理员' } ]

        onFail: opts.onFail// err 是错误信息

    })

}
```
8、选部门
```
tMobileSDK.selectDept = function(opts) {

    td.selectDept({

        onSuccess: opts.onSuccess,//返回值result是所选的部门信息： [ { deptid:0, deptName:'销售部门' } ]

        onFail: opts.onFail// err 是错误信息

    })

}
```

9、打电话
```
tMobileSDK.call = function(opts) {

    td.call({

        phoneNum: opts.phoneNum || '',//电话号码

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
10、发短信
```
tMobileSDK.sendMessage = function(opts) {

    td.sendMessage({

        phoneNums: opts.phoneNums || [''],//电话号码

        content: opts.content || '',//短信内容

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
11、获取网络类型
onSuccess返回值result为wifi, 2g, 3g, 4g, none, unknow
```
tMobileSDK.getNetworkType = function(opts) {

    td.getNetworkType({

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
12、选择图片
```
tMobileSDK.chooseImage = function(opts) {

    td.chooseImage({

        onSuccess: opts.onSuccess,//返回值result是所选图片的服务器地址 

        onFail: opts.onFail//返回值 err 是错误信息

    })

}
```
13、选择文件
```
tMobileSDK.selectFile = function(opts) {

    td.selectFile({

        onSuccess: opts.onSuccess,//返回值result是所选文件的服务器地址 

        onFail: opts.onFail//返回值err 是错误信息

    })

}
```
14、MAC地址
```
tMobileSDK.getMacAddress = function(opts) {

    td.getMacAddress({

        onSuccess: opts.onSuccess,
        onFail: opts.onFail

    })

}
```
15、地图
```
tMobileSDK.getLocationByMap = function(opts) {

    td.getLocationByMap({

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
16、图片预览
```
tMobileSDK.previewImage = function(opts) {

    opts.urls.forEach(function(v, k){

        opts.urls[k] = tMobileSDK.addAuthCode(v);

    });

    opts.current = tMobileSDK.addAuthCode(opts.current);

    td.previewImage({

        urls: opts.urls || [],//图片路径

        current: opts.current || 0,//当前图片索引

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
17、文件预览
```
tMobileSDK.previewFile = function(opts) {

    td.previewFile({

        url: opts.url || '',//文件路径

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```

18、设置客户端顶栏中间标题，此方法可以不直接调用，设置模块的顶栏见下方buildHeader方法 
```
tMobileSDK.setTitle = function(opts) {

    td.setTitle({

        title: opts.title || '标题',//控制标题文本，空字符串表示显示默认文本，需要左右标题的话用英文逗号进行分隔

        onSuccess: opts.onSuccess,

        onFail: opts.onFail

    })

}
```
19、设置客户端顶栏右侧按钮，此方法可以不直接调用，设置模块的顶栏见下方buildHeader方法 
```
tMobileSDK.setRight = function(opts) {

    td.setRight({

        show: opts.show || true,//控制按钮显示， true 显示， false 隐藏， 默认true

        control: opts.control || false,//是否控制点击事件，true 控制，false 不控制， 默认false

        text: opts.text || '',//控制显示文本，空字符串表示显示默认文本

        onSuccess: opts.onSuccess,//返回值result如果control为true，则onSuccess将在发生按钮点击事件被回调

        onFail: opts.onFail

    })

}
```
20、动作列表
```
tMobileSDK.actionSheet = function(opts) {

    td.actionSheet({

        title: opts.title || '标题',//标题

        cancelButton: opts.cancelButton || '取消',//取消按钮文本

        otherButtons: opts.otherButtons || ['btn1','btn2'],

        onSuccess: opts.onSuccess,// onSuccess将在点击button之后回调，被点击按钮的索引值，Number，从0开始, 取消按钮为-1

        onFail: opts.onFail

    })

}
```
21、设置客户端顶栏的左中右部分文字和对应的点击事件
```
//顶栏左中右对应的文字及事件
var headerData= {
        "page_info": {
            "l": {
                "class": "",
                "event": "history.back();",
                "title": ""
            },
            "c": {
                "title": "标题"
            },
            "r": {
                "class": "",
                "event": "",
                "title": "操作"
            }
        },
        ......
}

tMobileSDK.buildHeader = function(headerData) {
    var hash = {}
    var title = []
    if(headerData.c && headerData.c.push) {
        headerData.c.forEach(function(item, index) {
            title.push(item.title)
            hash[index] = item.event
        })
    } else if(headerData.c) {
        title.push(headerData.c.title)
        //hash[0] = headerData.c.event
    }

    headerData.c && this.setTitle({
        title: title,
        onSuccess: function(results) {
            if(results) {
                tMobileSDK.util.eval(hash[results])
            }
        }
    })
    if(headerData.r === null) headerData.r = {}
    headerData.r && this.setRight({
        show: true,
        control: true,
        text: headerData.r.title,
        onSuccess: function(results) {
            if(results === 'clicked') {
                headerData.r.event && tMobileSDK.util.eval(headerData.r.event)
            }
        }
    })
}
```
22、设置点击按钮要执行的动作列表
```
//funcData动作列表对应的文字和回调函数
            [
                {
                    'title': '保存',
                    'event': ''
                },
                {
                    'title': '删除',
                    'event': ''
                }
            ]

tMobileSDK.buildFunc = function(funcData) {
    var hash = {};
    var otherButtons = [];
    funcData.forEach(function(item, index) {
        hash[index] = item.event;
        otherButtons.push(item.title);
    })
    this.actionSheet({
        title: '选项：',
        otherButtons: otherButtons,
        onSuccess: function(results) {
            if(results != -1) {
                tMobileSDK.util.eval(hash[results])
            }
        }
    })
}
```

23、增加授权地址，@param url 地址
```
tMobileSDK.addAuthCode = function(url){

    return url;

}
```
