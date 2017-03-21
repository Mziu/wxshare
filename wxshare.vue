# wxshare
微信分享
<template>
	<div>
		微信分享测试{{msg}}
	</div>
</template>
<script>
import { mapActions } from 'vuex'
export default {
  name: 'wxshare',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      appId: '',
      timestamp: '',
      nonceStr: '',
      signature: '',
      r: ''
    }
  },
  methods: {
    ...mapActions(['getJsWeiXinConfig']),
    toGetwxConfig () {
      this.getJsWeiXinConfig().then(res => {
        this.r = res.object
        /* eslint-disable no-undef */
        wx.config({
          debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
          appId: res.object.appId, // 必填，公众号的唯一标识
          timestamp: this.r.timestamp, // 必填，生成签名的时间戳
          nonceStr: this.r.nonceStr, // 必填，生成签名的随机串
          signature: this.r.signature, // 必填，签名，见附录1
          jsApiList: ['onMenuShareTimeline', 'onMenuShareAppMessage'] // 分享到朋友圈 分享给朋友
        })
      })
    }
  },
  mounted () {
    this.toGetwxConfig()
    /* eslint-disable no-undef */
    wx.ready(function () {
     // 获取“分享到朋友圈”按钮点击状态及自定义分享内容接口
      wx.onMenuShareTimeline({
        title: '你奶奶个大姨妈',
        link: 'http://www.huaxiaohong.com',
        imgUrl: 'http://odsbdg1pr.bkt.clouddn.com/favin.png',
        success: function () {
          alert('success')
        },
        cancel: function () {
          alert('cancel')
        }
      })
      // 获取“分享给朋友”按钮点击状态及自定义分享内容接口
      wx.onMenuShareAppMessage({
        title: 'Hello world', // 分享标题
        desc: 'Hello world', // 分享描述
        link: '',
        imgUrl: 'http://odsbdg1pr.bkt.clouddn.com/favin.png', // 分享图标
        type: '' // 分享类型,music、video或link，不填默认为link
      })
    })
    wx.error(function () {
      alert('失败')
    })
  }
}
</script>
<style>
	
</style>
