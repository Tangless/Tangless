<template>
<div class="note">
    <div class="left-nav">
        <p class="title">笔记</p>
        <ul class="left-nav-list">
            <li class="note-menu active" v-on:click="choosePage($event)">
                <a class="menu-title" href="#">工作实践</a>
            </li>
            <!---->
            <li class="note-menu" v-on:click="choosePage($event)">
                <a class="menu-title" href="#">学习笔记</a>
            </li>
            <!---->
            <li class="note-menu" v-on:click="choosePage($event)">
                <a class="menu-title" href="#">杂记</a>
            </li>
        </ul>
    </div>
    <div class="right-content">
        <button class="new-article">新建文章</button>
        <div class="page-0" v-if="notePage=='0'">
            <table id="lstBox" cellspacing="0">
                <tbody>
                    <tr class="">
                        <th class="tdleft">标题</th>
                        <th>状态</th>
                        <th>阅读</th>
                        <th>评论</th>
                        <th>评论权限</th>
                        <th>操作</th>
                    </tr>
                    <tr class="">
                        <td class="tdleft">
                            <a href="http://blog.csdn.net/u014155882/article/details/69568625" target="_blank">CSS样式上下左右渐变出现</a>
                            <span class="gray">（2017-04-07 16:20）</span>
                        </td>
                        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                        <td>305</td>
                        <td>0</td>
                        <td>
                            <a href="?t=lock&amp;id=69568625" class="lock">禁止评论</a>
                        </td>
                        <td>
                            <a href="/postedit/69568625">编辑</a> | 
                            <a href="?t=top&amp;id=69568625">置顶</a> | 
                            <a href="?t=del&amp;id=69568625" name="del">删除</a> | 
                            <a href="javascript:void(0);" onclick="javascript:return setcat(this,69568625);" class="cat">分类</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="page-1" v-else-if="notePage=='1'">1</div>
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
                notePage:'0'
            }
        },
        methods:{
            choosePage:function(event){
                let el = event.currentTarget;
                $(".note-menu").removeClass("active");
                $(el).addClass("active");
                let text = $(el).find(".menu-title").text();
                switch (text){
                    case "工作实践":this.notePage="0";break;
                    case "学习笔记":this.notePage="1";break;
                    case "杂记":this.notePage="2";break;
                }
                // alert(this.notePage);
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