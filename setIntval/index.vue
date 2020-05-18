<template>
    <div class="timer">
        <div v-for="(item,index) in list" :key="item.id" @click="changeStatus(item,index)">
            {{item.id}}
           <span>{{item.timer}}</span>
        </div>
    </div>
</template>
<script>
const list = [
    {
        id:"一",
        oldTimer:null, // 距离现在24小时之后的时间戳
        timer:null
    },
    {
        id:"二",
        oldTimer:null,
        timer:null
    },
    {
        id:"三",
        oldTimer:null,
        timer:null
    }
]
export default {
    data(){
        return{
            list:list,
            aleradyClick:false
        }
    },
    created() {
        this.checkClickStatus()
    },
    methods: {
        checkClickStatus(){
            if(localStorage.getItem('aleradyClick')){
                this.list = JSON.parse(localStorage.getItem('list'))
                console.log('%c 🥛 localStorage.getItem(list): ', 'font-size:20px;background-color: #33A5FF;color:#fff;', this.list);
                this.setTimer()
            }
        },
        changeStatus(item,index){
            this.aleradyClick = true
            localStorage.setItem('aleradyClick',this.aleradyClick)
            let _this = this
            this.list.forEach((jtem,jndex) =>{
                if(index === jndex){
                    jtem.oldTimer =  jtem.oldTimer == null ? _this.getOldDate() : jtem.oldTimer
                    jtem.timer = _this.result(jtem.oldTimer)
                    _this.setTimer(jtem)
                }
            })
        },
        setTimer(){
            setInterval(() =>{
               this.updateTime()
            },1000)
        },
        updateTime(){
            this.list.forEach((el,index) => {
                if(this.list[index].timer !== null){
                    this.list[index].timer = this.result(el.oldTimer)
                }else{
                    this.list[index].timer = null
                }
                
            })
            localStorage.setItem('list',JSON.stringify(this.list))
        },
        getOldDate(){
            const timestamp=new Date().getTime() + 24 * 60 * 60 * 1000;
            return timestamp
        },
        result(oldTimer){
            const nowTime = new Date().getTime()
            const t = (oldTimer - nowTime) / 1000
            const d = parseInt(t / (60 * 60 * 24));
            const h = parseInt(t / 60 / 60 % 24);
            const m = parseInt(t / 60 % 60);
            const s = parseInt(t % 60);
            return d + "天" + h + "小时" + m + "分钟" + s + "秒";
        }
    },
}
</script>
