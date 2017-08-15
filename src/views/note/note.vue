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
        <button class="new-article">新建文章</button>
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
        <div class="page-1" v-else-if="this.$route.params.noteId=='study'">1</div>
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
            }
        }
    }     
</script>