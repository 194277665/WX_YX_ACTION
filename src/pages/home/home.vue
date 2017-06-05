<template>
    <div class="container">
        <mt-header title="行为记录">
            <!--<button slot="right" @click="handleClose">-->
                添加
            <mt-button slot="right" @click="handleClose">添加</mt-button>
            <!--</button>-->
        </mt-header>
        <!--<mt-search v-model="value" cancel-text="取消" placeholder="搜索"  show>-->
            <div class="cellDiv" v-for="(item,index) in actionItems" @click.stop.prevent="selectComups(index)">
                <div class="line"> <span>行为类型:</span> <span>{{item.XWLXDM_DISPLAY}}</span></div>
                <div class="line"><span>记录人:</span><span>{{item.YXRYID_DISPLAY}}</span></div>
                <div class="line"><span>日期:</span><span>{{item.SJ}}</span></div>
            </div>
        <!--</mt-search>-->


    </div>
</template>
<style scoped>
    body {
        background-color: #000000;
    }
    .container{
        background: #f9f9f9;
    }
    .cellDiv{
        width: 90%;
        height: auto;
        background: #ffffff;
        /*margin-top: 1PX;*/
        border-bottom: #f9f9f9 solid 1PX;
        padding: 10PX;
        /*align-content: center;*/
        margin:10PX auto;
        padding: 10PX;
    }
    .line{
        padding: 5PX;

    }
</style>
<script>
    import { Header, Search, Indicator, Button } from 'mint-ui';
    import $ from 'jquery'
    export default{
        created(){
            var searurl = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/yddxwjllbcx.do';
//            var searurl = '/sys/yxgl/modules/yddyxgl/yddxwjllbcx.do';
            Indicator.open();
            $.post(searurl,(data)=> {
                Indicator.close();
                this.actionItems = data.datas.yddxwjllbcx.rows;

            });

        },
        methods:{
            handleClose() {
                alert(11);
              this.$router.push('/index');

            },
        },
        data(){
            return{
              actionItems:[],
            }
        },

        components: {
            [Search.name]: Search,
            [Indicator.name]: Indicator,
            [Header.name]:Header,
            [Button.name]:Button
        },
        watch:{
            value:function (newValue,oldValue) {
                this.serarch(newValue);
            }
        },
    }
</script>