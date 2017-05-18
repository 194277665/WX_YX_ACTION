<template>
    <div class="content">
        <div>
            <span>支持人员：</span>
            <input type="text" value="" class="ZCRY"/>
        </div>
        <div>
            <span>支持内容：</span>
            <input type="text" value="" class="ZCNR"/>
        </div>
        <div>
            <span>时间：</span>
            <!--<input type="text" value="" class="SJ" />-->
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
        bottom: 20PX;
        margin-top: 20PX;
    }

    input {
        width: 100%;
        border: solid 1PX #f9f9f9;
        height: 30PX;
    }
</style>

<script type="text/javascript">
    import {Button, Toast, Indicator} from 'mint-ui';
    import userInfo from '../../userInfo';
    import Vue from 'vue'
    import MuseUI from 'muse-ui'
    import 'muse-ui/dist/muse-ui.css'
    Vue.use(MuseUI)
    import $ from 'jquery'
    export default {
        created(){
        },
        methods: {
            submit: function () {
                var ZCRY = $('.ZCRY').val();
                var ZCNR = $('.ZCNR').val();

                if(ZCRY.length == 0 || ZCNR.length == 0 || this.selectDate.length == 0)
                {
                    Toast('请完善提交信息');
                    return;
                }

                var param = {
                    WID: null,
                    XWLXDM: '5',
                    ZCRY: ZCRY,
                    ZCNR: ZCNR,
                    SJ: this.selectDate,
                    YXRYID: '',
                }

//               var  Url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/T_XWJL_XSZC_ADD.do';
                var Url = './modules/yddyxgl/T_XWJL_XSZC_ADD.do';
                Indicator.open();
                $.post(Url, param, function (data) {
                    Indicator.close();
                    if (data.code == 0) {
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