# citySelect微信小程序城市定位与选择

一、主要功能

1）默认定位到当前城市

2）用户可以自由切换城市和筛选。


二、准备工作

1）用户需要到腾讯定位服务设置key参数：https://lbs.qq.com/console/mykey.html

2）用户需要再微信公众号平台设置服务器域名，添加request合法域名：https://apis.map.qq.com

3）用户需要了解一下组件的使用，包括如何传值和获取选中的值。

  参考地址：https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html

4）需要在app.json中允许使用地理位置，配置permission。

 "permission": {
 
    "scope.userLocation": {
    
      "desc": "你的位置信息将用于小程序的接口展示"
      
    }
    
  }



三、使用

1）首页用户需要调用微信接口wx.getSetting来判断是否授权

    wx.getSetting({

          success: (res) => {

           //使用res.authSetting['scope.userLocation']字段来判断是否获取定位

           //只用res.authSetting['scope.userLocation']==true 时才是授权成功

          }

    })

2)调用wx.getLocation接口来获取当前位置经纬度，传给腾讯地图接口解析处城市。

  注意：使用腾讯地图接口需要引入qqmap-wx-jssdk.js，同时实例化
  
  /***引入腾讯接口文件并实例化**/  

  var QQMapWX = require('qqmap-wx-jssdk.js');

  var qqmapsdk;

  qqmapsdk = new QQMapWX({

    key: '腾讯地图Key值'

  });


   /***解析城市定位**/
   
   wx.getLocation({

       type: 'wgs84',

       success: function (res) {

         qqmapsdk.reverseGeocoder({

           location: {

             latitude: res.latitude,

             longitude: res.longitude

           },
           success: function (res) {

             let cityName=res.result.ad_info.city  //城市名称

           },

           fail: function (res) {

             // console.log(res);

           },
           complete: function (res) {

             // console.log(res);

           }

         });

       },
    
    fail: function (res) {
    
      console.log('fail' + JSON.stringify(res))
      
    }
    
  })

