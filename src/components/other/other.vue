<template>
    <div class="content">
        <div>
            <span>事项主题：</span>
            <input type="text" value="" class="SXZT"/>
        </div>
        <div>
            <span>事项内容：</span>
            <input type="text" value="" class="SXNR"/>
        </div>
        <div>
            <span>事项说明：</span>
            <input type="text" value="" class="SXSM"/>
        </div>
        <div>
            <span>时间：</span>
            <!--<input type="text" value="" class="SJ"/>-->
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
                var SXZT =$('.SXZT').val();
                var SXNR =$('.SXNR').val();
                var SXSM =$('.SXSM').val();
                if(SXZT.length == 0 || SXNR.length == 0 || this.selectDate.length == 0 ||SXSM.length == 0)
                {
                    Toast('请完善提交信息');
                    return;
                }
                var  param ={
                    WID:'',
                    XWLXDM:6,
                    SXZT:SXZT,
                    SXNR:SXNR,
                    SXSM:SXSM,
                    SJ:this.selectDate,
                    YXRYID:'',
                }

//                var Url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/T_XWJL_QT_ADD.do';
                var Url = './modules/yddyxgl/T_XWJL_QT_ADD.do';
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
        data(){
            return {
                selectDate: '',
            }
        },
        components: {
            [Button.name]: Button,
        },
    }
</script>