<template>
    <div class="demo">
        <div class="left-nav">
            <p class="title">项目</p>
            <ul class="left-nav-list">
                <router-link :to="{ name: 'index', params: { demoId: 'shili' }}">
                    <li class="note-menu demo-tab" v-bind:class="{ active: this.$route.params.demoId=='shili' }">
                        <a class="menu-title " href="#">示例</a>
                    </li>
                </router-link>
                <!---->
                <router-link :to="{ name: 'index', params: { demoId: 'gobang' }}">
                    <li class="note-menu demo-tab " v-bind:class="{ active: this.$route.params.demoId=='gobang' }">
                        <a class="menu-title " href="#">五子棋</a>
                    </li>
                </router-link>
                <!---->
                <router-link :to="{ name: 'index', params: { demoId: 'tuling' }}">
                    <li class="note-menu demo-tab" v-bind:class="{ active: this.$route.params.demoId=='tuling' }">
                        <a class="menu-title " href="#">图灵</a>
                    </li>
                </router-link>
            </ul>
        </div>
        <div class="right-content">
            <div class="page-0" v-if="this.$route.params.demoId=='shili'">0</div>
            <div class="page-1" v-else-if="this.$route.params.demoId=='gobang'">
                <div class="set-init" v-on:click="drawBag">初始化棋盘</div>
                <canvas width="1024" id="canvas" v-on:mousedown="clickEvent($event)" height="768"></canvas>
                <div class="gobang hide"
                     style="position: relative;display:block;top: -90px;background-color:#339999; padding:15px 114px;margin:0 0;width:1024px;height:50px;">
                    <div v-on:mousedown="goback($event)"
                         style="display: inline;float: left; padding:15px 50px;margin:0px; border:1px solid #000;color: #fff;font-size: 16px;">
                        悔棋
                    </div>
                    <div id="code"
                         style="display: inline;float: left;font-size: 25px ;padding:8px 194px;margin:0px;color:#cc0000 ">
                        黑棋走
                    </div>
                    <div v-on:mousedown="regoback($event)"
                         style="display: inline;float: right;padding:15px 50px;margin:0px; border:1px solid #000;color: #fff;font-size: 16px;">
                        撤销悔棋
                    </div>
                </div>
            </div>
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
                        <button class="send" v-on:keyup.up="send" v-on:click="send" @keyup.enter="send">send</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    /**全局参数初始化
     *
     */
    var canvas; //html5画布
    var context;
    var isWhite = false; //设置是否该轮到白棋，黑棋先手
    var winner = ''; //赢家初始化为空
    var step = 225;//总步数
    var initChess = new Array(15); //二维数组存储棋盘落子信息,初始化数组initChess值为0即此处没有棋子，1为白棋，2为黑棋
    for (var x = 0; x < 15; x++) {
        initChess[x] = new Array(15);
        for (var y = 0; y < 15; y++) {
            initChess[x][y] = 0;
        }
    }
    var remberData = new Array();//空数组，记录所有点击过的地方，为悔棋准备
    var clickNum = -1;//总的点击事件
    export default {
        // name:'demoList',
        data() {
            return {
                author: '11111',
            }
        },
        created() {
            this.keyupEnter()
        },
        methods: {
            keyupEnter() {
                document.onkeydown = e => {
                    let body = document.getElementsByTagName('body')[0]
                    if (e.keyCode === 13 && e.target != body) {
                        this.send();
                    }
                }
            },
            send: function () {
                let that = this;
                let msg = $(".edit-input").val();
                let url = '';
                let appkey = 'cb8d9ff9f9aa434aa85cea9a46106338';
                if (msg) {
                    $(".content-list").append(that.pushMsg(msg, true));
                    url = 'http://www.tuling123.com/openapi/api?key=cb8d9ff9f9aa434aa85cea9a46106338&info=' + msg + '&userid=000001';
                    $(".edit-input").val('');
                    $.ajax({
                        url: url,
                        type: 'POST',
                        success: function (data) {
                            let dataMsg = "";
                            switch (data.code) {
                                case 100000:
                                    //文本
                                    dataMsg = data.text;
                                    break;
                                case 200000:
                                    //有链接
                                    dataMsg = '<a target="_blank" href="' + data.url + '">' + data.text + '</a>';
                                    break;
                                case 302000:
                                    //新闻
                                    dataMsg = '<li class="news">' + '<a target="_blank" href="' + data.detailurl + '">' + data.article + '</a>' + '</li>';
                                    break;
                                case 308000:
                                    //菜谱
                                    for (let i = 0; i < data.list.length; i++) {
                                        dataMsg = dataMsg + '<ul class="caipu">' + '<li>菜谱: ' + data.list[i].name + '</li><li><img src="' + data.list[i].icon + '" /></li><li>材料：' + data.list[i].info + '</li><br>' + '<a target="_blank" href="' + data.list[i].detailurl + '">' + '查看详情</a>' + '</ul><br>';
                                    }
                                    break;

                            }
                            $(".content-list").append(that.pushMsg(dataMsg, false));
                        }
                    });

                }
            },
            /*拼接消息*/
            pushMsg: function (msg, isSelfSend) {
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
                    let d = $('.content-list');
                    let dh = d[0].scrollHeight;
                    d.scrollTop(dh);
                }, 100)
                return onemsg;
            },
            /*初始化棋盘*/
            drawBag: function () {
                $('.gobang').removeClass('hide');
                //创建棋盘背景
                canvas = document.getElementById("canvas");
                context = canvas.getContext("2d");
                context.fillStyle = '#339999';
                context.fillRect(0, 0, 1024, 768);
                //标题
                //context.fillStyle = '#00f';
                //context.font = '40px Arial';
                //context.strokeText('五子棋', 330, 50);
                /*//悔棋
                context.strokeRect(790,100,30,20);
                context.fillStyle='#000000';
                context.font='15px Arial';
                context.strokeText('悔棋',790,115);
                //撤销悔棋
                context.strokeRect(840,100,60,20);
                context.fillStyle='#000000';
                context.font='15px Arial';
                context.strokeText('撤销悔棋',840,115);
                    */
                //棋盘纵横线
                for (var i = 1; i < 16; i++) {
                    context.beginPath();
                    context.moveTo(40 * i + 140, 80);
                    context.lineTo(40 * i + 140, 640);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(180, 40 * i + 40);
                    context.lineTo(740, 40 * i + 40);
                    context.closePath();
                    context.stroke();
                }
            },
            /**悔棋点击事件*/
            goback: function (e) { //鼠标点击时发生
                let that = this;
                if (clickNum >= 0) {
                    //后退一步
                    that.clear(remberData[clickNum].color, remberData[clickNum].x, remberData[clickNum].y);
                    isWhite = !isWhite;
                    initChess[remberData[clickNum].x][remberData[clickNum].y] = 0;
                    step++;
                    clickNum--;
                    that.showColor();
                }
            },
            /**撤销悔棋点击事件*/
            regoback: function (e) { //鼠标点击时发生
                let that = this;
                if (clickNum < remberData.length - 1) {
                    clickNum++;
                    that.drawChess(remberData[clickNum].color, remberData[clickNum].x, remberData[clickNum].y);
                }
            },
            /**鼠标点击事件*/
            clickEvent: function (e) { //鼠标点击时发生
                let that = this;
                let color;
                var e = e || event;
                var px = e.clientX - 160 - 296;//减去160是因为canvas画布中棋盘第一根线距离左边是180的距离,再以每个横纵交点为圆心20px的范围内点击视为有效点击;296是棋盘距离左边的px
                var py = e.clientY - 60 - 162;//同上；142是棋盘距离上边的px
                var x = parseInt(px / 40);
                var y = parseInt(py / 40);
                if (px < 0 || py < 0 || x > 14 || y > 14 || initChess[x][y] != 0) { //鼠标点击棋盘外的区域不响应
                    return;
                }
                that.check(x, y);
            },
            /*check*/
            check: function (x, y) {
                let that = this;
                let color;
                if (winner != '' && winner != null) { //已经结束的游戏
                    setTimeout(function () {
                        alert(winner);
                    }, 500);
                    return;
                }
                if (isWhite) {
                    color = "white";
                } else {
                    color = "black";
                }
                console.log(color + "落子的位置是：" + x + "," + y);
                that.drawChess(color, x, y);
                var len = remberData.length;
                if (clickNum == len - 1) {
                    remberData.push({color: color, x: x, y: y});
                    clickNum++;
                } else if (clickNum < len - 1) {
                    clickNum++;
                    remberData[clickNum].color = color;
                    remberData[clickNum].x = x;
                    remberData[clickNum].y = y;
                    for (var index = clickNum + 1; index <= len - 1; index++) {
                        initChess[remberData[index].x][remberData[index].y] = 0;
                    }
                    remberData.splice(clickNum + 1, len - clickNum);
                }
            },
            /** 落子*/
            drawChess: function (color, x, y) { //参数为，棋（1为白棋，2为黑棋），数组位置
                let that = this;
                if (x >= 0 && x < 15 && y >= 0 && y < 15) {
                    if (color == "white") {
                        that.chess("white", x, y);
                        that.isWin("white", x, y); //判断输赢
                        isWhite = false;
                    } else {
                        that.chess("black", x, y);
                        that.isWin("black", x, y); //判断输赢
                        isWhite = true;
                    }
                }
                that.showColor();
                if (--step == 0) {
                    winner = "和局";
                    setTimeout(function () {
                        alert(winner);
                    }, 500);
                }
            },
            /**根据颜色和传入的坐标,绘制棋子*/
            chess: function (color, x, y) {
                context.fillStyle = color; //绘制黑棋
                context.beginPath();
                context.arc(x * 40 + 180, y * 40 + 80, 15, 0, Math.PI * 2, true);
                context.closePath();
                context.fill();
                if (color == "white") {
                    console.log("电脑在" + x + "," + y + "画了个白棋");//console.log仅仅是为了调试方便
                    initChess[x][y] = 1;//chessDate存储棋盘棋子的信息
                } else {
                    console.log("电脑在" + x + "," + y + "画了个黑棋");
                    initChess[x][y] = 2;
                }
            },
            /**清除棋子*/
            clear: function (color, x, y) {
                //背景
                context.fillStyle = '#339999'; //绘制背景色的圆
                context.beginPath();
                context.arc(x * 40 + 180, y * 40 + 80, 16, 0, Math.PI * 2, true);
                context.closePath();
                context.fill();
                //纵横线
                if ((x == 0) && (y > 0 && y < 14)) {
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 60);
                    context.lineTo(40 * x + 180, 40 * y + 100);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 80);
                    context.lineTo(40 * x + 200, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else if ((x == 14) && (y > 0 && y < 14)) {
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 60);
                    context.lineTo(40 * x + 180, 40 * y + 100);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 160, 40 * y + 80);
                    context.lineTo(40 * x + 180, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else if (x == 0 && y == 0) {
                    //左上角
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 80);
                    context.lineTo(40 * x + 180, 40 * y + 100);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 80);
                    context.lineTo(40 * x + 200, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else if (x == 14 && y == 0) {
                    //右上角
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 80);
                    context.lineTo(40 * x + 180, 40 * y + 100);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 160, 40 * y + 80);
                    context.lineTo(40 * x + 180, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else if (x == 0 && y == 14) {
                    //左下角
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 60);
                    context.lineTo(40 * x + 180, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 80);
                    context.lineTo(40 * x + 200, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else if (x == 14 && y == 14) {
                    //右下角
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 60);
                    context.lineTo(40 * x + 180, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 160, 40 * y + 80);
                    context.lineTo(40 * x + 180, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else if ((y == 0) && (x > 0 && x < 14)) {
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 80);
                    context.lineTo(40 * x + 180, 40 * y + 100);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 160, 40 * y + 80);
                    context.lineTo(40 * x + 200, 40 * y + 80);
                    context.closePath();
                    context.stroke();

                } else if ((y == 14) && (x > 0 && x < 14)) {
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 60);
                    context.lineTo(40 * x + 180, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 160, 40 * y + 80);
                    context.lineTo(40 * x + 200, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                } else {
                    context.beginPath();
                    context.moveTo(40 * x + 180, 40 * y + 60);
                    context.lineTo(40 * x + 180, 40 * y + 100);
                    context.closePath();
                    context.stroke();
                    context.beginPath();
                    context.moveTo(40 * x + 160, 40 * y + 80);
                    context.lineTo(40 * x + 200, 40 * y + 80);
                    context.closePath();
                    context.stroke();
                }
            },
            /**判断此局游戏是否已有结果
             * 每次落子判断游戏是否胜利*/
            isWin: function (color, x, y) {
                let that = this;
                console.log("判断" + color + "(" + x + "," + y + ")是否胜利");
                var temp = 2; //默认为黑色
                if (color == "white") {
                    temp = 1;
                } //白色
                console.log("temp=" + temp);
                that.horizontalCount(temp, x, y);
                that.verticalCount(temp, x, y);
                that.rightTopCount(temp, x, y);
                that.rightBottomCount(temp, x, y);
            },
            horizontalCount: function (temp, x, y) {
                let that = this;
                //横向判断
                var line = new Array(4);
                var count = 0;
                for (var i = x; i >= 0; i--) {
                    line[0] = i;
                    line[1] = y;
                    if (initChess[i][y] == temp) {
                        ++count;
                    } else {
                        i = -1;
                    }
                }
                for (var i = x; i <= 14; i++) {
                    line[2] = i;
                    line[3] = y;
                    if (initChess[i][y] == temp) {
                        ++count;
                    } else {
                        i = 100;
                    }
                }
                that.success(line[0], line[1], line[2], line[3], temp, --count);
            },

            verticalCount: function (temp, x, y) {
                let that = this;
                //竖向判断
                var line = new Array(4);
                var count = 0;
                for (var i = y; i >= 0; i--) {
                    line[0] = x;
                    line[1] = i;
                    if (initChess[x][i] == temp) {
                        ++count;
                    } else {
                        i = -1;
                    }
                }
                for (var i = y; i <= 14; i++) {
                    line[2] = x;
                    line[3] = i;
                    if (initChess[x][i] == temp) {
                        ++count;
                    } else {
                        i = 100;
                    }
                }
                that.success(line[0], line[1], line[2], line[3], temp, --count);
            },

            rightTopCount: function (temp, x, y) {
                let that = this;
                //右上斜判断'/'
                var line = new Array(4);
                var count = 0;

                for (var i = x, j = y; i <= 14 && j >= 0;) {
                    line[0] = i;
                    line[1] = j;
                    if (initChess[i][j] == temp) {
                        ++count;
                    } else {
                        i = 100;
                    }
                    i++;
                    j--;
                }
                for (var i = x, j = y; i >= 0 && j <= 14;) {
                    line[2] = i;
                    line[3] = j;
                    if (initChess[i][j] == temp) {
                        ++count;
                    } else {
                        i = -1;
                    }
                    i--;
                    j++;
                }
                that.success(line[0], line[1], line[2], line[3], temp, --count);
            },

            rightBottomCount: function (temp, x, y) {
                let that = this;
                //右下斜判断'\'
                var line = new Array(4);
                var count = 0;

                for (var i = x, j = y; i >= 0 && j >= 0;) {
                    line[0] = i;
                    line[1] = j;
                    if (initChess[i][j] == temp) {
                        ++count;
                    } else {
                        i = -1;
                    }
                    i--;
                    j--;
                }
                for (var i = x, j = y; i <= 14 && j <= 14;) {
                    line[2] = i;
                    line[3] = j;
                    if (initChess[i][j] == temp) {
                        ++count;
                    } else {
                        i = 100;
                    }
                    i++;
                    j++;
                }
                that.success(line[0], line[1], line[2], line[3], temp, --count);
            },
            /**判断是否胜利及胜利之后的操作*/
            success: function (a, b, c, d, temp, count) {
                if (count == 5) { //因为落子点重复计算了一次
                    console.log("此局游戏结束啦");
                    console.log("(" + a + "," + b + ")" + "到" + "(" + c + "," + d + ")");

                    context.beginPath();
                    context.lineWidth = 5;
                    context.strokeStyle = 'purple';
                    context.moveTo(40 * a + 180, 40 * b + 80);
                    context.lineTo(40 * c + 180, 40 * d + 80);
                    context.closePath();
                    context.stroke();


                    winner = "黑棋胜利!";
                    if (temp == 1) {
                        winner = "白棋胜利!";
                    }
                    setTimeout(function () {
                        alert(winner);
                    }, 500);

                }
            },
            /**显示下一种颜色*/
            showColor: function () {
                if (isWhite) {
                    document.getElementById("code").innerText = "白棋走"
                } else {
                    document.getElementById("code").innerText = "黑棋走"
                }
            }
            /**/
        }
    }
</script>
<style>
    .set-init {
        background-color: #4785f9;
        color: #FFF;
        outline: none;
        border: none;
        height: 30px;
        width: 80px;
        border-radius: 4px;
        margin-bottom: 20px;
        text-align: center;
        line-height: 30px;
    }
</style>