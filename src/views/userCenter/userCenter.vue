<template>
<div class="userCenter">
    <div class="left-nav">
        <p class="title">个人中心</p>
        <ul class="left-nav-list">
            <router-link :to="{ name: 'user', params: { userId: 'jieshao' }}">
                <li class="note-menu user-tab"  v-bind:class="{ active: this.$route.params.userId=='jieshao' }">
                    <a class="menu-title" href="#">介绍</a>
                </li>
            </router-link>
            <router-link :to="{ name: 'user', params: { userId: 'beian' }}">
                <li class="note-menu user-tab"  v-bind:class="{ active: this.$route.params.userId=='beian' }">
                    <a class="menu-title" href="#">网站备案</a>
                </li>
            </router-link>
            <router-link :to="{ name: 'user', params: { userId: 'download' }}">
                <li class="note-menu user-tab"  v-bind:class="{ active: this.$route.params.userId=='download' }">
                    <a class="menu-title" href="#">模板下载</a>
                </li>
            </router-link>
        </ul>
    </div>
    <div class="right-content">
        <div class="page-0" v-if="this.$route.params.userId=='jieshao'">
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
        <div class="page-1" v-else-if="this.$route.params.userId=='beian'">1</div>
        <div class="page-2" v-else>2</div>
    </div>
</div>
</template>

<script>
    export default {
        data() {
            return {
                author: '11111',
            }
        }
    }     
</script>