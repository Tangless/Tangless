<template>
<div class="userCenter">
    <div class="left-nav">
        <p class="title">个人中心</p>
        <ul class="left-nav-list">
            <li class="note-menu active" v-on:click="choosePage($event)">
                <a class="menu-title" href="#">介绍</a>
            </li>
            <li class="note-menu" v-on:click="choosePage($event)">
                <a class="menu-title" href="#">网站备案</a>
            </li>
            <li class="note-menu" v-on:click="choosePage($event)">
                <a class="menu-title" href="#">模板下载</a>
            </li>
        </ul>
    </div>
    <div class="right-content">
        <div class="page-0" v-if="userPage=='0'">
            <div class="resume-1">
                <div class="user-info">
                    <img src=""/>
                    <div class="user-name text-center">Tangless</div>
                    <div class="user-target text-center"></div>

                    <div class="user-mingzhu info-list text-left"><span class="icon-user"></span>民族：汉族</div>
                    <div class="user-birth info-list text-left"><span class="icon-Tender"></span>生日：1994.04.26</div>
                    <div class="user-jiguan info-list text-left"><span class="icon-card"></span>籍贯：重庆市渝北区</div>
                    <div class="user-address info-list text-left"><span class="icon-location-pcs"></span>现居：深圳市南山区</div>
                    <div class="user-mobile info-list text-left"><span class="icon-phone06"></span>电话：18381665312</div>
                    <div class="user-email info-list text-left"><span class="icon-envelope"></span>邮箱：782817837@qq.com</div>
                </div>
                <div class="user-demand">
                    <div class="user-education">
                        <span class="glyphicon glyphicon-education"></span>
                        <span class="title">教育信息</span>
                        <div class="description">
                            <span class="time">2012.09——2016.06</span>
                            <span class="address">西南科技大学</span>
                            <span class="major">电子信息工程</span>
                            <span class="job">本科</span>
                        </div>
                        <div class="detail">
                            <span >主修课程：</span>
                            <p>文件管理 数字处理,在极客文化的熏陶下，每一个加入公司的成员，都必须是行业里面的翘楚精英，他们坚信只有人才是我们改变这个世界的钥匙。很多时候，他们只想与众不同更贴心的服务而已。</p>
                        </div>
                    </div>
                    <div class="user-work">
                        <span class="glyphicon glyphicon-briefcase"></span>
                        <span class="title">工作经验</span>
                        <div class="description">
                            <span class="time">2016.07——至今</span>
                            <span class="address"></span>
                            <span class="job"></span>
                        </div>
                        <div class="detail">
                            <span >工作描述：</span>
                            <p></p>
                        </div>
                    </div>
                    <div class="user-tech">
                        <span class="glyphicon glyphicon-cog"></span>
                        <span class="title">专业技能</span>
                    </div>
                    <div class="user-tech">
                        <span class="glyphicon glyphicon-leaf"></span>
                        <span class="title">自我评价</span>
                    </div>
                </div>
            </div>
        </div>
        <div class="page-1" v-else-if="userPage=='1'">1</div>
        <div class="page-2" v-else>2</div>
    </div>
</div>
</template>

<script>
    export default {
        // name:'demoList',
        data() {
            return {
                author: '11111',
                userPage:'0'
            }
        },
        methods:{
            choosePage:function(event){
                let el = event.currentTarget;
                $(".note-menu").removeClass("active");
                $(el).addClass("active");
                let text = $(el).find(".menu-title").text();
                switch (text){
                    case "介绍":this.userPage="0";break;
                    case "网站备案":this.userPage="1";break;
                    case "模板下载":this.userPage="2";break;
                }
                // alert(this.userPage);
            },
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
                    userhead = '<img class="user-head-icon right" src="../../views/images/tulinguser.png"/>';
                    //小三角
                    /*<img class="triangle" src="../views/images/triangle.png">*/
                    triangle = '<i class="tr-sanjiao"></i>';

                    onemsg = '<div class="one-message self-msg">' + userhead +
                        '<div class="msgbody"><pre>' + msg + '</pre>' + triangle +
                        '</div></div>';

                }
                //不是自己发的消息
                else {
                    userhead = '<img class="user-head-icon left" src="../../views/images/tuling.png"/>';
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