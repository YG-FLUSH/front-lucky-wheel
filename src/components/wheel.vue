<template>
    <div class="wrapper typo" id="wrapper">

        <link rel="stylesheet" href="http://icono-49d6.kxcdn.com/icono.min.css">
        <h1>{{ title }}</h1>
        <div class="gb-wheel-container" id="gbWheel">
            <div class="gb-wheel-content gb-wheel-run">
                <ul class="gb-wheel-line">
                    <li v-for="award in awards"
                        :key="award.id"
                        class="gb-wheel-litem"
                        :style="{transform: award.line_rotate}"
                        >
                    </li>
                </ul>

                <div class="gb-wheel-list">
                    <div v-for="award in awards"
                         :key="award.id"
                         class="gb-wheel-item" >
                         <div class="gb-wheel-icontent"
                              :style="{transform: award.rotate}">
                             <p class="gb-wheel-iicon">
                                 <i :class="award.icon"></i>
                             </p>
                             <p class="gb-wheel-itext">
                                 {{ award.name }}
                             </p>
                         </div>
                    </div>
                </div>
            </div>
            <a href="javascript:;" class="gb-wheel-btn" id="gbLottery"
                                                        @click="lottery($event)">抽奖</a>
        </div>
    </div>
</template>

<script>
import NebPay from 'nebpay'

var nebPay = new NebPay();
var serialNumber = "";
var enableDebug = true;
function onPayClick () {
    var to = 'n1VzbMJ5mGMHRtG5apozSKunqxaKT88PNgJ';
    var value = 0.01;
    serialNumber = nebPay.pay(to, value, {
        qrcode: {
            showQRCode: false
        },
        goods: {
            name: 'luck wheel lottery',
            desc: 'run wheel'
        },
        listener: function(resp) {
            console.log("resp: " + JSON.stringify(resp));
            var i = 0;
            setInterval(()=>{
                console.log("resp timeout:  " + JSON.stringify(resp));
                i += 1;
                onrefreshClick(i);
                }, 1000);
        },
        debug: enableDebug,
    });
}

function onrefreshClick(i) {
    nebPay.queryPayInfo(serialNumber,{debug: enableDebug})
    .then(function (resp) {
        console.log(resp);
        var runEl = document.querySelector('.gb-wheel-run');
        runEl.style['transform'] = 'rotate(' + (360*3*i)+'deg)';
        runEl.style['-webkit-transition'] = "-webkit-transform 1.5s linear";
        runEl.style['transition'] = "transform 1.5s linear";
        console.log(runEl.style['transform']);
    })
    .catch(function (err) {
        console.log(err);
    });
}

function genAwardsRotate (awards) {
    awards.forEach(function (v, i, a) {
        var r = (2 * i) / 12
        v['rotate'] = 'rotate(' + r + 'turn)'
    })
}
function genLineRotate (awards) {
    awards.forEach(function (v, i, a) {
        var r = (2 * i + 1) / 12
        v['line_rotate'] = 'rotate(' + r + 'turn)'
    })
}
var awards = [
    {'id': 0, 'name': '耳机', 'icon': 'icono-headphone'},
    {'id': 1, 'name': 'iPhone', 'icon': 'icono-iphone'},
    {'id': 2, 'name': '相机', 'icon': 'icono-camera'},
    {'id': 3, 'name': '咖啡杯', 'icon': 'icono-cup'},
    {'id': 4, 'name': '日历', 'icon': 'icono-calendar'},
    {'id': 5, 'name': '键盘', 'icon': 'icono-keyboard'}
]
genAwardsRotate(awards)
genLineRotate(awards)

export default {
    name: 'wheel',
    data () {
        return {
            title: '幸运大轮盘',
            awards: awards
        }
    },
    methods: {
        lottery: function (event) {
            onPayClick()
        }
    }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.gb-wheel-container ul,
.gb-wheel-container li,
.gb-wheel-container p {
    margin: 0;
    padding: 0;
}

.gb-wheel-container ul,
.gb-wheel-container li {
    list-style: none;
}

.gb-wheel-container {
    margin: 0 auto;
    position: relative;
    width: 30rem;
    height: 30rem;
    overflow: hidden;
    border-radius: 50%;
}
.gb-wheel-content {
    position: absolute;
    left: 1rem;
    top: 1rem;
    width: 28rem;
    height: 28rem;
    box-sizing: border-box;
    border-radius: inherit;
    background-clip: padding-box;
    background: -webkit-radial-gradient(rgba(100, 100, 100, .1) 15%, transparent 16%) 0 0, -webkit-radial-gradient(rgba(100, 100, 100, .1) 15%, transparent 16%) 8px 8px, -webkit-radial-gradient(rgba(255, 255, 255, .1) 15%, transparent 20%) 0 1px, -webkit-radial-gradient(rgba(255, 255, 255, .1) 15%, transparent 20%) 8px 9px;
    background: radial-gradient(rgba(100, 100, 100, .1) 15%, transparent 16%) 0 0, radial-gradient(rgba(100, 100, 100, .1) 15%, transparent 16%) 8px 8px, radial-gradient(rgba(255, 255, 255, .1) 15%, transparent 20%) 0 1px, radial-gradient(rgba(255, 255, 255, .1) 15%, transparent 20%) 8px 9px;
    background-color: #ffcb3f;
    background-size: 12px 14px;
}

.gb-wheel-content::before {
    content: ' ';
    position: absolute;
    left: -1rem;
    top: -1rem;
    z-index: -1;
    width: 28rem;
    height: 28rem;
    border-radius: inherit;
    border: 1rem solid #E44025;
    box-shadow: 0 0 2px 2px rgba(0, 0, 0, .2) inset;
}
.gb-wheel-line{
    position: absolute;
    left: 0;
    top: 0;
    width: inherit;
    height: inherit;
    z-index: 99;
}
.gb-wheel-litem{
    position: absolute;
    left: 14rem;
    top: 0;
    text-align: center;
    width: 1px;
    height: 14rem;
    background-color: rgba(228, 55, 14, .6);
    -webkit-transform-origin: 50% 14rem;
}
.gb-wheel-list{
    position: absolute;
    left: 0;
    top: 0;
    width: inherit;
    height: inherit;
    z-index: 99;
}
.gb-wheel-icontent{
    position: relative;
    margin: 0 auto;
    padding-top: 1.5rem;
    text-align: center;
    -webkit-transform-origin: 50% 14rem;
    -ms-transform-origin: 50% 14rem;
    transform-origin: 50% 14rem;
}
.gb-wheel-item{
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    color: #e4370e;
    font-weight: bold;
    text-shadow: 0 1px 1px rgba(255, 255, 255, .6);
}
.gb-wheel-itext {
    font-size: 1.4rem;
    font-weight: lighter;
}

.gb-wheel-iicon [class*=icono-] {
    color: #e4370e;
}
.gb-wheel-btn{
    position: absolute;
    left: 11rem;
    top: 11rem;
    z-index: 999;
    width: 8rem;
    height: 8rem;
    margin: 0 auto;
    border-radius: 50%;
    color: #F4E9CC;
    background-color: #E44025;
    line-height: 8rem;
    text-align: center;
    font-size: 2rem;
    text-shadow: 0 -1px 1px rgba(0, 0, 0, .6);
    box-shadow: 0 3px 5px rgba(0, 0, 0, .6), 0 0 5px 4px rgba(0, 0, 0, .2) inset;
    text-decoration: none;
}

a.gb-wheel-btn {
    border-bottom: none;
}
a:hover {
    color: #a0a0a0;
    text-shadow: 0 -1px 1px rgba(0,0,0,0.6);
}

.gb-wheel-btn::after {
    position: absolute;
    content: '';
    left: 2.5rem;
    top: -1rem;
    width: 3rem;
    height: 3rem;
    background-color: #E44025;
    -webkit-transform: rotate(45deg);
    -ms-transform: rotate(45deg);
    transform: rotate(45deg);
    box-shadow: 0 3px 5px rgba(0, 0, 0, .6), 0 0 5px 4px rgba(0, 0, 0, .2) inset;
}

.gb-wheel-btn.disabled,
.gb-wheel-btn.disabled::after {
    pointer-events: none;
    background: #B07A7B;
    color: #ccc;
}

.gb-wheel-run {
}
</style>
