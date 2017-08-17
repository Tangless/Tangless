<template>
    <header id="header">
        <!--{{childCom}}-->
        <!--<div @click="open">hhh</div>-->
        <div class="nav-wrapper gradient-line">
            <div class="nav-content clearfix">
                <div class="pull-left">
                    <a href="/" class="nav-logo">
                        <img src="static/images/logo.png">
                        <span class="logo-text">TangLess</span>
                    </a>
                </div>
                <div class="nav pull-right">
                    <ul id="J_nav" class="clearfix nav-list">
                        <router-link :to="{ name: 'index', params: { demoId: 'shili' }}">
                            <li id="demo" class="nav-item" v-on:click="chooseTab($event)" v-bind:class="{ hover: hoverClass=='0' }">
                                <a class="nav-item-link" href="#">项目</a>
                                <ul class="nav-sub-list fadeUp">
                                    <router-link :to="{ name: 'index', params: { demoId: 'shili' }}"><li class="nav-sub-item"><a href="../index.html">示例</a></li></router-link>
                                    <router-link :to="{ name: 'index', params: { demoId: 'gobang' }}"><li class="nav-sub-item"><a href="../index.html">五子棋</a></li></router-link>
                                    <router-link :to="{ name: 'index', params: { demoId: 'tuling' }}"><li class="nav-sub-item"><a href="../index.html">图灵</a></li></router-link>
                                </ul>
                            </li>
                        </router-link>
                        <router-link :to="{ name: 'note', params: { noteId: 'work' }}">
                            <li id="note" class="nav-item" v-on:click="chooseTab($event)" v-bind:class="{ hover: hoverClass=='1' }">
                                <a class="nav-item-link" href="#">笔记</a>
                                <ul class="nav-sub-list fadeUp">
                                    <router-link :to="{ name: 'note', params: { noteId: 'work' }}"><li class="nav-sub-item"><a href="../note/index.html">工作实践</a></li></router-link>
                                    <router-link :to="{ name: 'note', params: { noteId: 'study' }}"><li class="nav-sub-item"><a href="../note/index.html">学习笔记</a></li></router-link>
                                    <router-link :to="{ name: 'note', params: { noteId: 'other' }}"><li class="nav-sub-item"><a href="../note/index.html">杂记</a></li></router-link>
                                </ul>
                            </li>
                        </router-link>
                        <router-link :to="{ name: 'user', params: { userId: 'jieshao' }}">
                            <li id="userCenter" class="nav-item last" v-on:click="chooseTab($event)" v-bind:class="{ hover: hoverClass=='2' }">
                                <a class="nav-item-link" href="#">个人中心</a>
                                <ul class="nav-sub-list fadeUp">
                                    <router-link :to="{ name: 'user', params: { userId: 'jieshao' }}"><li class="nav-sub-item"><a href="../userCenter/index.html">介绍</a></li></router-link>
                                    <router-link :to="{ name: 'user', params: { userId: 'beian' }}"><li class="nav-sub-item"><a href="../userCenter/index.html">网站备案</a></li></router-link>
                                    <router-link :to="{ name: 'user', params: { userId: 'download' }}"><li class="nav-sub-item"><a href="../userCenter/index.html">模板下载</a></li></router-link>
                                </ul>
                            </li>
                        </router-link>
                    </ul>
                </div>
            </div>
        </div>
    </header>
</template>

<script>
    export default {
        // name: 'my-header',
        data() {
            return {
                author: 'Tangless',
                hoverClass:"0"
            }
        },
        props:{
            childCom:{
                type: String,
                default: 'sichaoyun' 
            }
        },
        methods:{
            open:function(){
                this.$emit('showbox','the msg'); //触发showbox方法，'the msg'为向父组件传递的数据
            },
            chooseTab:function(event){
                let el = event.currentTarget;
                let text = $(el).find(".nav-item-link").text();
                switch (text){
                    case "项目":this.hoverClass="0";break;
                    case "笔记":this.hoverClass="1";break;
                    case "个人中心":this.hoverClass="2";break;
                }
                globalData.status.choosePage = this.hoverClass;
                // alert(this.pageIndex);
            },
            changeNote:function(event){
                let el = event.currentTarget;
                let text = $(el).find("a").text();
                switch (text){
                    case "工作实践":globalData.status.noteClass="0";break;
                    case "学习笔记":globalData.status.noteClass="1";break;
                    case "杂记":globalData.status.noteClass="2";break;
                }
            }
        }
    } 
</script>