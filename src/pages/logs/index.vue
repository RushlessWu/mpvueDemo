<template>
  <view>
    <image :src="userInfo.avatarUrl"></image>
    <text class='nickName'>{{userInfo.nickName}}</text>
    <van-panel title="预约信息"  status="预约成功">
      <view class="appointment">
        <van-cell-group :border="false">
          <van-cell title="预约号" :value="appointment.id" />
          <van-cell title="姓名" :value="appointment.name" />
          <van-cell title="电话" :value="appointment.tel" />
          <van-cell title="医馆地址" :value="appointment.address" />
          <van-cell title="预约时间" :value="appointment.timer" />
          <van-cell title="备注" :value="appointment.message" />
        </van-cell-group>
      </view>
    </van-panel>
    <van-panel title="预约信息"  status="暂无信息" style="margin-boootm:20px">
      <view class="appointment">暂无预约内容</view>
    </van-panel>
  <!--<view class="telus">祝医生 13755330747 <van-button type="primary" size="mini" @click="freeTell">联系我们</van-button></view>-->
  </view>
</template>

<script>



export default {
 
  data () {
    return {
      userInfo: {},
      appointment:{
        id:"01",
        name:'张三',
        tel:'18679850212',
        address:'上东城',
        timer:'2020/10/24',
        message:''
      }
    }
  },

  created () {
    let that=this
    /* wx.getUserInfo({
      success: function(res) {
        that.userInfo=res.userInfo
      }
    }) */
    this.userInfo=wx.getStorageSync('globalData')
},
methods:{
  freeTell: function(){
    wx.makePhoneCall({
      phoneNumber: '13755330747',
    })
  }
}
}
</script>
<style>
image{
  width:132px;
  height:132px;
  display:block;
  margin:10px auto;
  border-radius: 100%;
}
.nickName{
  width:100%;
  display:block;
  text-align:center;
  margin: 20px 0;
}
.appointment {
  padding: 20px;
  font-size: 14px;
}
.telus{
  position: fixed;
  left:25%;
  bottom:0;
  font-size: 12px;
  color: #646566;
  text-align: center;
  margin: 20px 0;
  background:#fff;
}
</style>
