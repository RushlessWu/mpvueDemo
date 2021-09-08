<template>
  <div>
     <swiper v-if="imgUrls.length > 0" indidator-dots="imgUrls.length > 1" autoplay="true">
      <block v-for="(item, index) in imgUrls" :key="index" >
        <swiper-item>
          <image :src="item" mode="scaleToFill"></image>
        </swiper-item>
      </block>
    </swiper>
    <van-notice-bar
      left-icon="volume-o"
      :scrollable="true"
      text="学春堂温馨提示：疫情期间为了您和他人的健康，就诊请佩戴口罩,谢谢！"
    />
    <h2 class="title">挂号预约</h2>
    <van-cell-group>
      <van-field
        :value="username"
        required
        clearable
        label="用户名"
        placeholder="请输入用户名"
      />
      <van-field
        :value="tel"
        required
        clearable
        label="电话"
        placeholder="请输入电话号码"
      />
      <view class="selected">
      <van-row>
        <van-col span="8"><i style="color:#ee0a24;float:left;">*</i>医馆选择</van-col>
         <van-col span="16">
         <van-radio-group :value="radio" @change="onChange">
            <van-row style="margin-top:10px;display:block">
              <van-col span="12">
                <van-radio name="上东城店" icon-size="15px" checked-color="#07c160">上东城店</van-radio>
              </van-col>
              <van-col span="12">
                <van-radio name="煌固店" icon-size="15px" checked-color="#07c160">煌固店</van-radio>
              </van-col>
            </van-row>
          </van-radio-group>
         </van-col>
      </van-row>
      </view>
      <van-cell title-style="font-size:14px;color:#646566;" title="预约日期" :value="date" @click="onDisplay" is-link required/>
      <van-calendar title="预约日期" :show="show" @close="onClose" @confirm="onConfirm" color="#07c160"/>
      <van-field
    :value="message"
    label="备注"
    type="textarea"
    autosize
    placeholder="请输入备注"
  />
    </van-cell-group>
    <van-button type="primary" size="large" class="submit" @click="submit">提交</van-button>
    <van-dialog
    title="学春堂申请获取你的昵称、头像等信息"
    :show="showDialog"
    theme= 'round-button'
    confirmButtonText='允许授权'
    confirm-button-open-type="getUserInfo"
    @confirm="bindGetUserInfo"
  >
  </van-dialog>
  </div>
</template>

<script>

export default {
  data () {
    return {
       imgUrls: [
        'http://mss.sankuai.com/v1/mss_51a7233366a4427fa6132a6ce72dbe54/newsPicture/05558951-de60-49fb-b674-dd906c8897a6',
        'http://mss.sankuai.com/v1/mss_51a7233366a4427fa6132a6ce72dbe54/coursePicture/0fbcfdf7-0040-4692-8f84-78bb21f3395d',
        'http://mss.sankuai.com/v1/mss_51a7233366a4427fa6132a6ce72dbe54/management-school-picture/7683b32e-4e44-4b2f-9c03-c21f34320870'
      ],
      nickName:'',
      show: false,
      username:"",
      tel:"",
      date: '',
      radio:'上东城店',
     message:'',
showDialog:false
    }
  },

  methods: {
    bindViewTap () {
      const url = '../logs/main'
      if (mpvuePlatform === 'wx') {
        mpvue.switchTab({ url })
      } else {
        mpvue.navigateTo({ url })
      }
    },
     onDisplay() {
        this.show= true;
      },
    onClose() {
      this.show= false;
    },
    formatDate(date) {
      date = new Date(date);
      return `${date.getFullYear()}/${date.getMonth() + 1}/${date.getDate()}`;
    },
    onConfirm(event) {
        this.show= false,
        this.date= this.formatDate(event.mp.detail)
    },
    onChange(event) {
    this.radio= event.mp.detail
    },
    bindGetUserInfo(){
      let that=this
      let globalData=wx.getStorageSync('globalData')
    if(!globalData){
      wx.login({
        success: function () {
          wx.getUserInfo({
            success: function (res) {
              wx.setStorage({
                key:'globalData',
                data:res.userInfo
              })
              that.nickName=res.userInfo.nickName
            }
          })
        }
      })  
    }else{
      that.nickName=globalData.nickName
      that.showDialog=false
    }
    }
  },

  created () {
    // let app = getApp()
    
      this.showDialog=true
}
}
</script>

<style scoped>
swiper {
  height: 200px;
}
._image {
  width: 100%;
}
.title {
  font-size: 16px;
  margin: 10px 0 20px;
  text-align: center;
  font-weight: 700;
}
.submit {
  display: block;
  margin: 20px 0;
}
.selected{
  height: 44px;
  line-height: 44px;
  font-size: 14px;
  color:#646566;
  margin-left: 10px;
  border-bottom: 1px solid #ebedf0;
}

</style>
