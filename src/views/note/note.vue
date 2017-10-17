<template>
<div class="note">
    <div class="left-nav">
        <p class="title">笔记</p>
        <ul class="left-nav-list">
            <router-link :to="{ name: 'note', params: { noteId: 'work' }}">
                <li class="note-menu note-tab"  v-bind:class="{ active: this.$route.params.noteId=='work' }">
                    <a class="menu-title" href="#">工作实践</a>
                </li>
            </router-link>
            <!---->
            <router-link :to="{ name: 'note', params: { noteId: 'study' }}">
                <li class="note-menu note-tab"  v-bind:class="{ active: this.$route.params.noteId=='study' }">
                    <a class="menu-title" href="#">学习笔记</a>
                </li>
            </router-link>
            <!---->
            <router-link :to="{ name: 'note', params: { noteId: 'other' }}">
                <li class="note-menu note-tab"  v-bind:class="{ active: this.$route.params.noteId=='other' }">
                    <a class="menu-title" href="#">杂记</a>
                </li>
            </router-link>
        </ul>
    </div>
    <div class="right-content">
        <button class="new-article" v-on:click="createArticle">新建文章</button>
        <div class="page-0" v-if="this.$route.params.noteId=='work'">
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
                    <tr class="" v-for="item in lists">
                        <td class="tdleft">
                            <a v-bind:href="item.title_link" target="_blank">{{item.title}}</a>
                            <span class="gray">（2017-04-07 16:20）</span>
                        </td>
                        <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                        <td>{{item.read_number}}</td>
                        <td>{{item.comment_number}}</td>
                        <td>
                            <a href="?t=lock&amp;id=69568625" class="lock">禁止评论</a>
                        </td>
                        <td>
                            <a href="/postedit/69568625">编辑</a> | 
                            <a href="?t=top&amp;id=69568625">置顶</a> | 
                            <a href="javascript:void(0);" v-on:click="del">删除</a> | 
                            <a href="javascript:void(0);" onclick="javascript:return setcat(this,69568625);" class="cat">分类</a>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="page-1" v-else-if="this.$route.params.noteId=='study'">1</div>
        <div class="page-2" v-else>2</div>
    </div>
</div>
</template>

<script>

    // 初始化野狗云数据库
    var config = {
        authDomain: "wd1487810104sghfpv.wilddog.com",
        syncURL: "https://wd1487810104sghfpv.wilddogio.com"
    };
    wilddog.initializeApp(config);
    var ref = wilddog.sync().ref("/tangless_blog/files");

    /**读取数据，初始化列表*/
    var val = wilddog.sync().ref("/tangless_blog/files");
    var arr = [ ];
    
    function getLists(){
        arr = [ ]
        val.once('value', function(snapshot) {
        　　for ( var i in snapshot.val() ){
                snapshot.val()[i].id=i;
                var str = snapshot.val()[i];
                arr.push(str);
        　　}
        });
    }

    //事件监听
    ref.on('child_added', function(data) {
        console.log(data.val().author + " 发布了一篇名为《" + data.val().title + "》的博客");
        getLists();
        
        //按key排序
        // console.log(data.key());
    });
    ref.on('child_changed', function(data) {
        console.log(data.val().author + " 更新博客标题为《" + data.val().title + "》");
        getLists();
    
    });
    ref.on('child_removed', function(data) {
        console.log( "博客《" + data.val().title + "》被删除");
        getLists();
    
    });
    

    export default {
        // name:'demoList',
        data() {
            return {
                author: '11111',
                lists: arr
            }
        },
        methods:{
            /*新建文章*/
            createArticle: function() {
                
                //追加子节点
                var postsRef = ref;
                postsRef.push({
                    "title": "CSS样式上下左右渐变出现",//文章名
                    "title_link": "http://blog.csdn.net/u014155882/article/details/69568625",//文章地址
                    "author": "Tangless",//作者
                    "status": "true",//状态
                    "read_number": "0",//阅读数
                    "comment_number": "0",//评论数
                    "comment_authority": false,//评论权限
                    "content": ""//内容
                }, function(error) {
                    if (error == null){
                        alert("数据同步到野狗云成功");
                    }
                });
                
            },
            /**删除数据*/
            del: function() {
                ref.remove();
                // ref.on('child_removed', function(snapshot) {
                //     console.log("=============child_removed==========================");
                //     console.log(snapshot.child("sayword").val());
                //     snapshot.key().remove();
                // });
            },
            /**编辑数据*/
            edit: function() {
                //更新数据
                 
                //update 方法如果有key 该key不做任何变动；如果key不存在，则创建一个新的key
                //set 完全覆盖以前的所有
                // 只更新 files 的 title_name
                //files里面不是一组数组，更新时只更新了第一条
                var hopperRef = ref.child("files");
                hopperRef.update({
                    "title": "abb"
                });
                //同时更新 b 节点下的 d 和 x 节点下的 z：
                // hopperRef.update({
                //     "b/d": "updateD",
                //     "x/z": "updateZ"
                // });
            }
        }
    }     
</script>