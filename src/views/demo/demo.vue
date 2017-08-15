<template>
  <div class="demo">
    <div class="left-nav">
        <p class="title">项目</p>
        <ul class="left-nav-list">
            <router-link to="/index:shili">
            <li class="note-menu demo-tab" v-bind:class="{ active: this.$route.params.demoid==':shili' }">
                <a class="menu-title " href="#">示例</a>
            </li>
            </router-link>
            <!---->
            <router-link to="/index:api">
            <li class="note-menu demo-tab "  v-bind:class="{ active: this.$route.params.demoid==':api' }">
                <a class="menu-title " href="#">API</a>
                <!--<div>
                    <div class="menu-title ">API<span class="icon-circle-down"></span></div>
                    <ul class="sub-list" style="display: none;">
                        <li class="">
                            <a href="#">G2</a>
                        </li>
                        <li class="">
                            <a href="#">Chart</a>
                        </li>
                    </ul>
                </div>-->
            </li>
            </router-link>
            <!---->
            <router-link to="/index:tuling">
            <li class="note-menu demo-tab"  v-bind:class="{ active: this.$route.params.demoid==':tuling' }">
                <a class="menu-title " href="#">图灵</a>
            </li>
            </router-link>
        </ul>
    </div>
    <div class="right-content">
        <div class="page-0" v-if="this.$route.params.demoid==':shili'">0</div>
        <div class="page-1" v-else-if="this.$route.params.demoid==':api'">1</div>
        <div class="page-2" v-else>
            <div class="robot">
                <h1 class="text-center">天下事</h1>
                <div class="content-list">
                    <div class="one-message">
                        <div class="msgfrom-info">叮铃玲玲</div>
                        <img class="user-head-icon left" src="static/images/tuling.png">
                        <div class="msgbody">
                            <pre>说点儿什么吧</pre>
                            <i class="tl-sanjiao"></i>
                        </div>
                    </div>
                </div>
                <div class="edit text-left">
                    <input class="edit-input" placeholder="知天下事"/>
                    <button class="send" v-on:keyup.enter="send" v-on:click="send" >send</button>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
    export default {
        // name:'demoList',
        data() {
            return {
                author: '11111',
                // demoPage:'0'
            }
        },
        methods:{
            // choosePage:function(event){
            //     let el = event.currentTarget;
            //     $(".demo-tab").removeClass("active");
            //     $(el).addClass("active");
            //     let text = $(el).find(".menu-title").text();
            //     switch (text){
            //         case "示例":this.demoPage="0";break;
            //         case "API":this.demoPage="1";break;
            //         case "图灵":this.demoPage="2";break;
            //     }
            //     // alert(this.demoPage);
            // },
            send:function(){
                let that=this;
                let msg = $(".edit-input").val();
                let url = '';
                let appkey = 'cb8d9ff9f9aa434aa85cea9a46106338';
                if(msg){
                    $(".content-list").append(that.pushMsg(msg,true));
                    url='http://www.tuling123.com/openapi/api?key=cb8d9ff9f9aa434aa85cea9a46106338&info='+msg+'&userid=000001';
                    $(".edit-input").val('');
                    $.ajax({
                        url: url,
                        type: 'POST',
                        success: function (data) {
                            let dataMsg="";
                            switch (data.code){
                                case 100000:
                                //文本
                                    dataMsg=data.text;
                                    break;
                                case 200000:
                                //有链接
                                    dataMsg='<a target="_blank" href="'+data.url+'">'+data.text+'</a>';
                                    break;
                                case 302000:
                                //新闻
                                    dataMsg='<li class="news">' + '<a target="_blank" href="' + data.detailurl + '">' + data.article + '</a>' + '</li>';
                                    break;
                                case 308000:
                                //菜谱
                                    for(let i=0;i<data.list.length;i++){
                                        dataMsg=dataMsg+'<ul class="caipu">' + '<li>菜谱: ' + data.list[i].name +'</li><li><img src="'+data.list[i].icon+'" /></li><li>材料：'+data.list[i].info+'</li><br>'+ '<a target="_blank" href="' + data.list[i].detailurl + '">' + '查看详情</a>' + '</ul><br>';
                                    }
                                    break;

                            }
                            $(".content-list").append(that.pushMsg(dataMsg,false));
                        }
                    });

                }
            },
            /*拼接消息*/
            pushMsg:function(msg,isSelfSend){
                let triangle, userhead;
                let onemsg = '';
                let msghead = '';
                //如果自己发的消息
                if (isSelfSend) {
                    userhead = '<img class="user-head-icon right" src="static/images/tulinguser.png"/>';
                    //小三角
                    /*<img class="triangle" src="../views/images/triangle.png">*/
                    triangle = '<i class="tr-sanjiao"></i>';

                    onemsg = '<div class="one-message self-msg">' + userhead +
                        '<div class="msgbody"><pre>' + msg + '</pre>' + triangle +
                        '</div></div>';

                }
                //不是自己发的消息
                else {
                    userhead = '<img class="user-head-icon left" src="static/images/tuling.png"/>';
                    msghead = '<div class="msgfrom-info">叮铃玲玲</span></div>';
                        //小三角
                    // triangle = '<img class="triangle" src="../views/images/triangle2.png">';
                    triangle = '<i class="tl-sanjiao"></i>';
                    
                    onemsg = '<div class="one-message">' + msghead + userhead +
                        '<div class="msgbody"><pre>' + msg + '</pre>' + triangle +
                        '</div></div>';
                }
                //如果是图片,去掉<pre>标签
                if (onemsg.indexOf('chat-img') > 0) {
                    onemsg = onemsg.replace('<pre>', '').replace('</pre>', '');
                }

                /*滚动条居底*/
                setTimeout(function () {
                    let d=$('.content-list');
                    let dh= d[0].scrollHeight;
                    d.scrollTop(dh);
                },100)
                return onemsg;
            }
        }
    } 
    
</script>
