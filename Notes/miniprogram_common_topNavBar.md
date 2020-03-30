## 小程序通用顶部导航栏组件

### 功能：回退按钮、标题、自定义标题颜色、自定义背景色、自动适配不同机型顶部胶囊按钮。


主组件代码
	
	<!-- color:标题颜色 -->
	<!-- menuBtnHeight: 胶囊按钮高度 -->
	<!-- menuButtonTop: 胶囊按钮距离顶部高度 -->
	<!-- backgroundColor: 背景色 -->
	<view class="navbar clearfloat" 
		style='color:{{color}};
		height:{{menuBtnHeight}}rpx;
		margin-top:{{menuButtonTop}}rpx;
		line-height:{{menuBtnHeight}}rpx;
		background:{{backgroundColor}}'>
	
			<!-- showTitle:是否现实标题 -->
			<!-- title:标题 -->
    		<text wx:if='{{showTitle}}' class='title'>{{title}}</text>
  			<image src="../../static/icon/common_arrow_left.png" class="arrowLeft" wx:if='{{showBackBtn&&iconColor=="black"}}' bindtap='handleBack'></image>
    		<image src="../../static/icon/common_arrowleft_w.png" class="arrowLeft" wx:if='{{showBackBtn&&iconColor=="white"}}' bindtap='handleBack'></image>
	</view>
	
样式代码：

	.navbar {
	    width: 100%;
	    text-align: center;
	    line-height: 100%;
	    position: relative;
	}
	
	.navbar .title {
	    font-family: PingFangSC-Medium;
	    font-size: 36rpx;
	}
	
	.navbar .arrowLeft {
	    position: absolute;
	    top: 8rpx;
	    left: 20rpx;
	    width: 48rpx;
	    height: 48rpx;
	}
	
js代码

	Component({
	    properties: {
	        title: {
	            type: String,
	            value: '标题'
	        },
	        showTitle: {
	            type: Boolean,
	            value: true
	        },
	        showBackBtn: {
	            type: Boolean,
	            value: true,
	        },
	        backgroundColor: {
	            type: String,
	            value: '#fff',
	        },
	        iconColor: {
	            type: String,
	            value: 'black',
	        },
	        color: {
	            type: String,
	            value: '#333',
	        },
	    },
	    data: {
	        menuBtnHeight: 0,
	        menuButtonTop: 0,
	        routePath: '',
	        backNum: 1,
			delta: 1
	    },
	    options: {
	        multipleSlots: true
	    },
	    ready: function(options) {
	        let app = getApp()
	        <!-- 来自小程序加载时获取的全局数据 -->
	        this.setData({
	            menuBtnHeight: app.globalData.menuBtnHeight,
	            menuButtonTop: app.globalData.menuButtonTop,
	        })
	    },
	    methods: {
	        // 回退 未指定路由则为回退一级，指定路由则为导航到指定页面
	        handleBack: function() {
	            if (this.data.routePath != '') {
	                wx.redirectTo({
	                    url: this.data.routePath
	                })
	            } else {
	                wx.navigateBack({
						delta: this.data.delta
	                })
	            }
	        },
	        
	        <!-- 传入指定路由 -->
	        handleChangeRouter(url) {
	            this.setData({
	                routePath: url
	            })
	        },
			
			<!-- 修改回退等级 -->
			handleChangeDelta(num) {
				this.setData({
					delta: num
				})
			},
	    }
	})
	
页面尺寸信息在app.js中获取

	const UTILS = require('/utils/util.js')

	App({
	    onLaunch: function() {
	        // 获取系统信息
	        wx.getSystemInfo({
	            success: (res) => {
	                this.globalData.systemType = res.platform
	                let proportion = 750 / res.windowWidth,
	                menuButtonObject = wx.getMenuButtonBoundingClientRect() // 获取胶囊按钮信息
	                this.globalData.menuBtnHeight = menuButtonObject.height * proportion // 胶囊按钮高度
	                this.globalData.menuButtonTop = menuButtonObject.top * proportion // 胶囊按钮距离顶部高度
	                this.globalData.windowHeight = res.windowHeight * proportion // 窗口高度
	            }
	        })
	    },
	
	    globalData: {
	        menuBtnHeight: 0,
	        menuButtonTop: 0,
	        windowHeight: 0,
	        systemType: null,
	    }
	})
	
	