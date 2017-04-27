# APICloudLiveDemo
此项目是使用亲加APICloud SDK开发的直播Demo，包括了直播，美颜，连麦等功能  
## 亲加通讯云模块的使用
亲加通讯云包括了基础模块（gotyeLiveCore）、聊天室模块（gotyeLiveChat）、播放器模块（gotyeLivePlayer）、直播模块（gotyeLivePublisher）、连麦模块（gotyeLiveP2P）共五大模块，用户的角色，需要用到的模块也不同；
 ![image](https://github.com/QPlus/APICloudLiveDemo/raw/master/MDImages/modulelist.png)

* gotyeLiveCore为基础模块，需首先调用。通过该模块的authRoomSession接口验证房间信息。其中session参数的内容为上文中提到的房间ID、密码、昵称。验证通过后，根据回调返回的角色，再调用对应的后续模块
* gotyeLiveChat为聊天模块，若需要看到其它用户的聊天信息或者自己发出聊天内容，则需要使用该模块
   1. 接口调用顺序：先通过init接口初始化，然后再通过login接口登入。登录成功后，可根据需求设置监听
   2. 监听
   	 * receiveMsg：即可获取到房间其他用户所发出的聊天信息通知 
   	 * forceLogout：监听账号在其它设备登录后强制下线
    	 * 其它：更多监听可查看模块文档
* gotyeLivePlayer为播放器模块，若用户角色为助理或观众，使用该模块即可看到直播内容
   1. 接口调用顺序：先通过init接口初始化，然后再通过play接口播放直播画面
   2. 监听：可根据实际业务需求，监听connected、disconnected、error、liveStart、liveStop等事件并响应
* gotyeLivePublisher为直播模块，若用户角色为主播，使用该模块即可进行直播，以及直播时可使用到的摄像头前后切换、分享、横竖屏切换、闪光灯开启、美颜、静音等功能
   1. 接口调用顺序：先通过init接口初始化，然后再通过startPreview接口开启摄像头，开启成功后通过login接口登入。此时直播还未正式开始，若要开启直播，还需调用publish接口开启
   2. 监听：可根据实际业务需求，监听connected、disconnected、error等事件并响应
* gotyeLiveP2P为连麦模块，需由主播发起，观众或助理可选择接受连麦或拒绝连麦，接受后即建立连麦连接，可相互看到对方的画面
   1. 接口调用顺序：接受连麦请求后通过joinRoom接口建立连麦连接，之后可通过leaveRoom接口断开连麦连接
   2. 监听：可根据实际业务需求，监听joinedRoom、connected、disconnected、error等事件并响应
		
	您可通过http://docs.apicloud.com/Client-API/Open-SDK/gotyeLiveCore了解亲加通讯云模块的详细说明
## 开发注意事项
1. 在config.xml文件中添加模块配置
 ![image](https://github.com/QPlus/APICloudLiveDemo/raw/master/MDImages/config.png)
2. 直播页面开发


 ![image](https://github.com/QPlus/APICloudLiveDemo/raw/master/MDImages/filelist.png)
   
   上图为demo的工程结构，通过html目录可以看到，分别有p2p、player、player_mask、publisher、publisher_mask四个html文件。通过上面的模块说明，我们知道player和player_mask是针对助理及开发者的，publisher和publisher_mask是针对主播的。
   
   以player为例，player.html页面用来展示直播画面，通过gotyeLivePlayer的play接口的playView参数指定，该页需提前open，并且为frame类型。而player_mask.html用来展示直播画面的遮罩层的功能，比如观看人数、清晰度切换、聊天内容、发送聊天等。gotyeLivePlayer所有的接口都可在player_mask.html里调用
   
3. 连麦
	连麦的请求发起及回应，可通过gotyeLiveChat的sendMessage接口来实现。开发者需通过该接口的text及extra参数设置好内容，然后在消息监听里做判断及处理
	
4. 其他
	demo代码里只是展示了亲加通讯云的主要功能，开发者们可根据实际业务需要自行使用其它接口
    

 
