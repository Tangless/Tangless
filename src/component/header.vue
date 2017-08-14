<template>
    <header id="header">
        <div class="nav-wrapper gradient-line">
            <div class="nav-content clearfix">
                <div class="pull-left">
                    <a href="/" class="nav-logo">
                        <img src="https://zos.alipayobjects.com/rmsportal/aGpKpuxoGfZqJobMPLvj.svg">
                        <span class="logo-text">TangLess</span>
                    </a>
                </div>
                <div class="nav pull-right">
                    <ul id="J_nav" class="clearfix nav-list">
                        <router-link to="/index">
                            <li id="demo" class="nav-item" v-on:click="chooseTab($event)" v-bind:class="{ hover: hoverClass=='0' }">
                                <a class="nav-item-link" href="#">项目</a>
                                <ul class="nav-sub-list fadeUp">
                                    <li class="nav-sub-item"><a href="../index.html">示例</a></li>
                                    <li class="nav-sub-item"><a href="../index.html">API</a></li>
                                    <li class="nav-sub-item"><a href="../index.html">图灵</a></li>
                                </ul>
                            </li>
                        </router-link>
                        <router-link to="/note">
                            <li id="note" class="nav-item" v-on:click="chooseTab($event)" v-bind:class="{ hover: hoverClass=='1' }">
                                <a class="nav-item-link" href="#">笔记</a>
                                <ul class="nav-sub-list fadeUp">
                                    <li class="nav-sub-item" v-on:click="changeNote($event)"><a href="../note/index.html">工作实践</a></li>
                                    <li class="nav-sub-item" v-on:click="changeNote($event)"><a href="../note/index.html">学习笔记</a></li>
                                    <li class="nav-sub-item" v-on:click="changeNote($event)"><a href="../note/index.html">杂记</a></li>
                                </ul>
                            </li>
                        </router-link>
                        <router-link to="/user">
                            <li id="userCenter" class="nav-item last" v-on:click="chooseTab($event)" v-bind:class="{ hover: hoverClass=='2' }">
                                <a class="nav-item-link" href="#">个人中心</a>
                                <ul class="nav-sub-list fadeUp">
                                    <li class="nav-sub-item"><a href="../userCenter/index.html">介绍</a></li>
                                    <li class="nav-sub-item"><a href="../userCenter/index.html">网站备案</a></li>
                                    <li class="nav-sub-item"><a href="../userCenter/index.html">模板下载</a></li>
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
        methods:{
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