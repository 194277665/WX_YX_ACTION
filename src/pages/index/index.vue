<template>
    <div id="app" class="index">
        <mt-header title="行为记录"></mt-header>
        <div class="menu">
            <span>行为类型：</span>
            <!--<div class="select">-->
            <select v-model="selected">
                <option value="visit">客户拜访</option>
                <option value="meeting">公司会议</option>
                <option value="office">日常办公</option>
                <option value="activity">市场活动</option>
                <option value="sell">销售支持</option>
                <option value="other">其他</option>
            </select>
            <!--</div>-->
            <router-view></router-view>
        </div>
    </div>
</template>

<style scoped>
    body {
        background: #f9f9f9;
    }

    .index {
        background: #f9f9f9;
        font-size: 14PX;
        width: 100%;
        height: 100%;
    }

    .menu {
        padding: 10PX;
        background: #f9f9f9;
    }

    span {
        display: block;
        width: 100%;
        padding-top: 10PX;
        padding-bottom: 6PX;
    }

    select {
        border: solid #f9f9f9 1PX;
        width: auto;
        height: 30PX;
        background: white;
    }

    .select {
        border: solid #f9f9f9 1PX;
        width: auto%;
        height: 30PX;
        background: white;
    }

    /*router-view{*/
    /*background: #f9f9f9;*/
    /*}*/
</style>
<script type="text/javascript">
    import {Header} from 'mint-ui';
    import userInfo from '../../userInfo'
    import $ from 'jquery'

    export default {
        created() {

            this.$router.push("visit")

//            var url  = window.location.href;
//            this.getAccessToken();


        },
        methods: {

            parseQueryString(url){
                var obj = {};
                var start = url.indexOf("?") + 1;
                var str = url.substr(start);
                var arr = str.split("&");
                for (var i = 0; i < arr.length; i++) {
                    var arr2 = arr[i].split("=");
                    obj[arr2[0]] = arr2[1];
                }
                return obj;
            },


            getAccessToken(){

//                $.get('https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx04af0b8fa5159d15&redirect_uri=http://amptest.wisedu.com/pw/sys/yxgl/index.html&response_type=code&scope=snsapi_base&state=snsapi_base&connect_redirect=1#wechat_redirect',(data)=>{
//                   alert('----------'+data);
//                });
                $.get('https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=wx04af0b8fa5159d15&corpsecret=MCCeCR8Yndm7v8O4a4hqkm-UyxI-_Uir6ViKSue5u5sqfcVa80qjXEXfiq_ehCh6',
                    (data) => {
                        var acces_token = data.access_token;

                        var url  = window.location.href;
                        var obj = this.parseQueryString(url);
                        var code = obj.code;
//                        alert('code'+code);
//                        alert('token'+acces_token);
                        console.log('code'+code);
                        console.log('access_token'+data.access_token);
                        $.get('https://qyapi.weixin.qq.com/cgi-bin/user/getuserinfo', {access_token: acces_token,code:code}, (data) => {

                            userInfo.user_id = data.UserId;
                            console.log('user_id'+userInfo.user_id);
                            console.log('data'+data);
                            alert('id'+userInfo.user_id)
                            this.$router.push("visit")
                        });

                    })
            }

        },
        data(){
            return {
                selected: "visit"
            }
        },
        components: {
            [Header.name]: Header,
        },
        watch: {
            selected: function (val) {
                console.log(val);
                this.$router.push(val)
            }
        }
    }
</script>