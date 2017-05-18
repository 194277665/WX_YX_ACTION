<template>
    <div class="content">
        <div>
            <span>工作时间：</span>
            <mu-date-picker hintText="选择时间" v-model="selectDate"/>
        </div>

        <div>
            <span>工作地点：</span>
            <input type="text" value="" class="GZDD"/>
        </div>
        <div>
            <span>工作内容：</span>
            <input type="text" value="" class="GZNR"/>
        </div>
        <div>
            <span>工作成果：</span>
            <input type="text" value="" class="GZCG"/>
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
    import Vue from 'vue'
    import MuseUI from 'muse-ui'
    import 'muse-ui/dist/muse-ui.css'
    Vue.use(MuseUI)
    import $ from 'jquery'
    export default {
        methods:{
            submit:function () {
                var GZDD = $('.GZDD').val();
                var GZNR = $('.GZNR').val();
                var GZCG = $('.GZCG').val();
                if(GZDD.length == 0 || GZNR.length == 0 || this.selectDate.length == 0 ||GZCG.length == 0)
                {
                    Toast('请完善提交信息');
                    return;
                }

                var  param ={
                    WID:'',
                    XWLXDM:3,
                    SJ:this.selectDate,
                    GZDD:GZDD,
                    GZNR:GZNR,
                    GZCG:GZCG,
                    KHMC:'金智',//固定
                    YXRYID:'',

                }

//                var Url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/T_XWJL_RCBG_ADD.do';
                var Url = './modules/yddyxgl/T_XWJL_RCBG_ADD.do';
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
            return{
                selectDate:'',
            }

        },
        components: {
            [Button.name]: Button,
        },
    }
</script>