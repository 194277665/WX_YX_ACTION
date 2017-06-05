<template xmlns="http://www.w3.org/1999/html">
    <div class="content">
        <div>
            <span>客户名称：</span>
            <select v-model="KHBH">
                <option :value="item.KHBH" v-for="(item,index) in schools">{{item.KHMC}}</option>
            </select>
        </div>
        <div>
            <span>拜访对象：</span>
            <input type="text" value="" class="BFDX"/>

        </div>
        <div>
            <div class="div-inline">
                <span>是否存在风险：</span>
                <select v-model="FX">
                    <!--<option>其他</option>-->
                    <option value="1">是</option>
                    <option value="2">否</option>
                </select>
            </div>
            <div class="div-inline">
                <span>是否需要资源协助：</span>
                <select v-model="ZYXZ">
                    <option value="1">是</option>
                    <option value="2">否</option>
                </select>
            </div>
        </div>
        <div>
            <span v-show="FX == 1">风险详情：</span>
            <!--<input type="text" value="" class="FXXQ" v-show="FX == 1"/>-->
            <textarea value="" class="FXXQ" v-show="FX == 1"></textarea>
        </div>
        <div>
            <span v-show="ZYXZ == 1">所需协助内容：</span>
            <!--<input type="text" value="" class="SXXZNR" v-show="ZYXZ == 1"/>-->
            <textarea value="" class="SXXZNR" v-show="ZYXZ == 1"></textarea>
        </div>
        <div>
            <span v-show="ZYXZ == 1">协助人：</span>
            <input type="text" value="" class="XZRID" v-show="ZYXZ == 1"/>
        </div>
        <div>
            <span>开始时间：</span>
            <!--<input type="text" value="" class="SJ" />-->
            <mu-date-picker hintText="选择时间" v-model="startDate"/>
        </div>
        <div>
            <span>结束时间：</span>
            <!--<input type="text" value="" class="SJ" />-->
            <mu-date-picker hintText="选择时间" v-model="endDate"/>
        </div>
        <!--</div>-->
        <div class="div-button">
            <mt-button type="primary" size="small" @click="submit">提交</mt-button>
            <mt-button type="primary" size="small" @click="close">关闭</mt-button>
        </div>
    </div>
</template>

<style scoped>
    body {
        background: #f9f9f9;

    }

    .content {
        width: 100%;
        height: 100%;
        background: #f9f9f9;
    }

    .khmc {
        background: white;
        height: 30PX;

    }

    .div-inline {
        display: inline-block;
    }

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

    select {
        border: solid #f9f9f9 1PX;
        background: white;
        width: auto;
        height: 30PX;
    }

    input {
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
    import {Button, Toast, Indicator} from 'mint-ui';
    import $ from 'jquery';
    import userInfo from '../../userInfo'
    import Vue from 'vue'
    import MuseUI from 'muse-ui'
    import 'muse-ui/dist/muse-ui.css'
    Vue.use(MuseUI)
    export default {
        created(){
            this.FX = 1;
            this.ZYXZ = 1;
            var compusInfo = window.sessionStorage.getItem('compusInfo')
            if (compusInfo) {
                this.compusInfo = JSON.parse(compusInfo)
            }
            window.sessionStorage.removeItem('compusInfo')

//            var url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/xwjlkhcx.do';
            var url = './modules/yddyxgl/xwjlkhcx.do';
//            Indicator.open();
            $.post(url, (data) => {
//                Indicator.close();
                this.schools = data.datas.xwjlkhcx.rows;
                console.log(data);
            });


        },
        data(){
            return {
                startDate: '',
                endDate: '',
                KHBH: '',
                schools: [],
                selectDate: '',
                FX: '',
                ZYXZ: '',
                compusInfo: {
                    KHBH: '',
                    KHMC: ''
                }
            }
        },
        methods: {
            goToSearch: function () {
                this.$router.push({path: 'searchCompus', query: {name: 'visit'}});
            },

            submit: function () {
                var BFDX = $('.BFDX').val();
                var CYRY = $('.CYRY').val();
                var FXXQ = $('.FXXQ').val();
                var SXXZNR = $('.SXXZNR').val();
                var XZRID = $('.XZRID').val();
                if (BFDX.length == 0 || this.startDate.length == 0 || this.endDate.length == 0) {
                    Toast('请完善提交信息');
                    return;
                }
                if (this.FX == 1 && FXXQ.length == 0) {
                    Toast('请完善提交信息');
                    return;

                }


                if (this.ZYXZ == 1 && (SXXZNR.length == 0 || XZRID.length == 0)) {
                    Toast('请完善提交信息');
                    return;
                }


                console.log('dfdsfdsa' + this.KHBH);
                var param = {
                    WID: '',
                    XWLXDM: 1,
//                  KHJBXXID:this.compusInfo.KHBH,
                    KHJBXXID: this.KHBH,
                    BFDX: BFDX,
                    CYRY: '',
                    SFCZFXDM: this.FX,
                    SFXYZYXZDM: this.ZYXZ,
                    FXXQ: FXXQ,
                    SXXZNR: SXXZNR,
                    XZRID: XZRID,
                    KSSJ: this.startDate,
                    JSSJ: this.endDate,

//                  SJ:this.selectDate,
                }
                console.log(param);

                Indicator.open();
//                var url = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/T_XWJL_KHBF_ADD.do';
                let url = './modules/yddyxgl/T_XWJL_KHBF_ADD.do';
                $.post(url, param, function (data) {
                    Indicator.close();
                    if (data.code == 0) {
                        Toast('提交成功');
                        this.close();
                    }
                });
            },
            close(){
                this.$router.go(-2);

            }


        },
        components: {
            [Button.name]: Button,
        },
        watch: {}
    }
</script>