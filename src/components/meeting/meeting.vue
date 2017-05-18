<template>
    <div class="content">
        <div>
            <span>会议主题：</span>
            <input type="text" value="" class="HYZT"/>
        </div>
        <div>
            <span>与会人：</span>
            <input type="text" value="" class="YHR" />
        </div>
        <div>
            <span>会议内容：</span>
            <input type="text" value="" class="HYNR"/>
        </div>
        <div>
            <span>会议结论：</span>
            <input type="text" value="" class="HYJL"/>
        </div>
        <div>
            <span>会议时间：</span>

            <mu-date-picker hintText="选择时间" v-model="selectDate"/>
        </div>
        <div class="div-button">
            <mt-button type="primary" size="small" @click="submit">提交</mt-button>
            <mt-button type="primary" size="small">关闭</mt-button>
        </div>
    </div>
</template>

<style scoped>
    span {
        display: block;
        width: 100%;
        padding-top: 10PX;
        padding-bottom: 6PX;
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
    input{
        width: 100%;
        border: solid 1PX #f9f9f9;
        height: 30PX;
    }
</style>

<script type="text/javascript">
    import userInfo from '../../userInfo';
    import { Button, Toast, Indicator } from 'mint-ui';
    import Vue from 'vue'
    import MuseUI from 'muse-ui'
    import 'muse-ui/dist/muse-ui.css'
    Vue.use(MuseUI)
    import $ from 'jquery'
    export default {

        data(){
          return{
              WID:'',
              XWLXDM:2,
              HYZT:'',
              YHR:'',
              HYNR:'',
              HYJL:'',
              SJ:'',
              YXRYID:'',

              selectDate:'',
          }
        },
        components: {
            [Button.name]: Button,
        },
        methods:{
            submit:function () {
                var HYZT = $('.HYZT').val();
                var YHR = $('.YHR').val();
                var HYNR = $('.HYNR').val();
                var HYJL = $('.HYJL').val();

                if(HYZT.length == 0 || YHR.length == 0 || HYNR.length == 0 ||HYJL.length == 0 ||this.selectDate.length == 0)
                {
                    Toast('请完善提交信息');
                    return;
                }

                var  param ={
                WID:'',
                XWLXDM:2,
                HYZT:HYZT,
                YHR:YHR,
                HYNR:HYNR,
                HYJL:HYJL,
                SJ:this.selectDate,
                YXRYID:'',
            }

//             var Url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/T_XWJL_GSHY_ADD.do';
                var Url = './modules/yddyxgl/T_XWJL_GSHY_ADD.do';
                Indicator.open();
                $.post(Url,param,function (data) {
                    Indicator.close();
                    if(data.code == 0)
                    {
                        Toast('提交成功');
                    }
                });

        }

     },
        watch:{
            HYZT:function (newValue,oldValue)
            {
                this.HYZT = newValue;

            },
            YHR:function (newValue,oldValue)
            {
                this.YHR = newValue;
            },
            HYNR:function (newValue,oldValue)
            {
                this.HYNR = newValue;
            },
            HYJL:function (newValue,oldValue)
            {
                this.HYJL = newValue;
            },
            SJ:function (newValue,oldValue)
            {
                this.SJ = newValue;
            },

        }
    }
</script>