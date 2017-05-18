<template>
    <div>
        <!--<mt-header title="搜索学校">-->
            <!--<router-link to="/" slot="left">-->
                <!--<mt-button icon="back" @click="goback"></mt-button>-->
            <!--</router-link>-->
            <!--&lt;!&ndash;<mt-button icon="more" slot="right"></mt-button>&ndash;&gt;-->
        <!--</mt-header>-->
        <mt-search v-model="value" cancel-text="取消" placeholder="搜索"  show>
            <!--<div class="container">-->
            <div class="cellDiv" v-for="(item,index) in cumpuse" @click.stop.prevent="selectComups(index)">
                <span>{{item.KHMC}}</span>
            </div>
            <!--</div>-->
        </mt-search>


    </div>
</template>
<style scoped>
    body {
        /*background-color: #;*/
    }
    /*.container{*/
        /*width: 100%;*/
        /*height: 100%;*/
        /*background: #f9f9f9;*/

    /*}*/
    .cellDiv{
        width: 100%;
        height: auto;
        background: #f9f9f9;
        margin-top: 1PX;
        padding: 10PX;

    }
</style>
<script>
    import { Header, Search, Indicator, Button } from 'mint-ui';
    import $ from 'jquery'
    export default{
        created(){
            window.sessionStorage.removeItem('compusInfo')
        },
        methods:{
            goback(){
                this.$router.go(-1);
                alert(11);
            },
            selectComups (index) {
                var compusItem = this.cumpuse[index]
                window.sessionStorage.setItem('compusInfo', JSON.stringify(compusItem))
                 this.$router.push({
                     name: 'Visit',
                     query: {
                        KHBH: compusItem.KHBH
                    }
                 })
            },
            serarch:function (keyWord) {
//                var searurl = 'http://amptest.wisedu.com/pw/sys/yxgl/modules/yddyxgl/khbhmccx.do';
                var searurl = './modules/yddyxgl/khbhmccx.do';
                Indicator.open();
                console.log(keyWord);

                $.post(searurl,{KHMC:keyWord},(data)=> {
                    Indicator.close();

                  this.cumpuse = data.datas.khbhmccx.rows;

                });

            }

        },
        data(){
            return{
                value:'',
                items:[],
                cumpuse:[],
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