<template>
    <div class="content">
        <div>
            <span>活动主题：</span>
            <input type="text" value="" class="HDZT"/>
        </div>
        <div>
            <span>活动地点：</span>
            <input type="text" value="" class="HDDD"/>
        </div>
        <div>
            <span>与会人：</span>
            <input type="text" value="" class="YHR"/>
        </div>
        <div>
            <span>活动时间：</span>
            <!--<input type="text" value="" class="SJ"/>-->
            <mu-date-picker hintText="选择时间" v-model="selectDate"/>

        </div>
        <div>
            <span>活动成果：</span>
            <!--<input type="text" value="" class="HDCG"/>-->
            <textarea value="" class="HDCG"></textarea>
        </div>
        <div class="div-button">
            <mt-button type="primary" size="small" @click="submit">提交</mt-button>
            <mt-button type="primary" size="small">关闭</mt-button>
        </div>
    </div>
</template>

<style scoped>
    .content{
        color: #f9f9f9;
    }
    span {
        display: block;
        width: 100%;
        padding-top: 10PX;
        padding-bottom: 6PX;
        color: #030300;
    }

    .div-button {
        /*position:fixed;*/
        bottom:20PX;
        margin-top: 20PX;
    }
    input{
        width: 100%;
        border: solid 1PX #f9f9f9;
        height: 30PX;
    }
    textarea{
        width: 100%;
        height: 80PX;

    }
</style>

<script type="text/javascript">
    import userInfo from '../../userInfo';
    import { Button, Toast, Indicator } from 'mint-ui';
    import $ from 'jquery'
    import Vue from 'vue'
    import MuseUI from 'muse-ui'
    import 'muse-ui/dist/muse-ui.css'
    Vue.use(MuseUI)
    export default {
        methods:{
            submit:function () {
                var HDZT = $('.HDZT').val();
                var HDDD = $('.HDDD').val();
                var YHR = $('.YHR').val();
                var HDCG = $('.HDCG').val();
                if(HDZT.length == 0 || HDDD.length == 0 || YHR.length == 0 || this.selectDate.length == 0 ||HDCG.length == 0)
                {
                    Toast('请完善提交信息');
                    return;
                }
                var  param ={
                    WID:'',
                    XWLXDM:4,
                    HDZT:HDZT,
                    HDDD:HDDD,
                    YHR:YHR,
                    SJ:this.selectDate,
                    HDCG:HDCG,
                    YXRYID:'',

                }
//              var Url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/T_XWJL_SCHD_ADD.do';
                var Url = './modules/yddyxgl/T_XWJL_SCHD_ADD.do';
                Indicator.open();
                $.post(Url,param,function (data) {
                    Indicator.close();
                    if(data.code == 0)
                    {
                        Toast('提交成功');
                        this.close();
                    }
                });
            },
            close(){
                this.$router.go(-2);

            }

        },
        data(){

          return{
              selectDate:'',
          }
        },
        components: {
            [Button.name]: Button,
        },
    }
</script>