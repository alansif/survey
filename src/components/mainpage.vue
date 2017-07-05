<template>
    <div>
        <div style="width:200px;margin: 20px;">
            <el-input v-model="idnumber"></el-input>
            <el-input v-model="phone"></el-input>
            <el-input v-model="vcode"></el-input>
            <el-button @click="getverify">GetVerify</el-button>
            <el-button @click="getlogin">Login</el-button>
            <el-button @click="go">GetQuestion</el-button>
            <el-button @click="getpre">GetPreviousAnswer</el-button>
        </div>
        <div class="card">
            <div class="cardhead">
                <div style="margin-left:32px;font-size:20px;">调查问卷</div>
            </div>
            <div class="cardbody">
                <template v-for="(question,idx) in questions">
                    <p class="ques" :class="hint && noe(ans[question.QID]) ? 'errcls': 'okcls'" :id="question.QID">{{question.QID}}.{{question.Question}}</p>
                    <checkboxs v-if="question.Multiple" :items="question.Alts" :qid="question.QID" :curselext="ans[question.QID]" @change="changed"></checkboxs>
                    <radios v-else :items="question.Alts" :qid="question.QID" :curselext="ans[question.QID]" @change="changed"></radios>
                </template>
                <hr style="margin-top:32px;height:1px;border:none;border-top:1px solid #ccc;" />
                <div style="width:100%;margin-top:24px;text-align:center;">
                    <el-button type="primary" @click="nextstep">下一步</el-button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import radios from './radios.vue'
    import checkboxs from './checkboxs.vue'

    function scrollElement(elmnt, duration) {
        var e = document.documentElement;
        if(e.scrollTop===0){
            var t = e.scrollTop;
            ++e.scrollTop;
            e = t+1===e.scrollTop--?e:document.body;
        }
        scrollTo(e, elmnt.offsetTop, duration);
    }
    
    function scrollTo(element, to, duration) {
        if (duration <= 0) return;
        var difference = to - element.scrollTop;
        var perTick = difference / duration * 10;
        setTimeout(function() {
            element.scrollTop = element.scrollTop + perTick;
            if (element.scrollTop === to) return;
            scrollTo(element, to, duration - 10);
        }, 10);
    }
    
    export default {
        data () {
            return {
                idnumber:'',
                phone:'',
                vcode:'',
                questions: [],
                ans:{},
                hint:false
            }
        },
        methods: {
            noe(a) {
                return !a || (a.length === 0);
            },
            getverify() {
                this.$http.post("http://111.198.146.40:3939/customize/MyService.asmx/GetVerifyInfo",{SFZH:this.idnumber,Phone:this.phone})
                    .then((response)=>{
                        let d = response.body.d;
                        console.log(d);
                    }, (response)=>{
                        console.log(response);
                    })
                    .catch((response)=>{
                        console.log(response);
                    });
            },
            getlogin() {
                this.$http.post("http://111.198.146.40:3939/customize/MyService.asmx/GetCustomerLoginInfo",{PaperValue:this.idnumber,Mobile:this.phone,PWD:this.vcode})
                    .then((response)=>{
                        let d = response.body.d;
                        console.log(d);
                    }, (response)=>{
                        console.log(response);
                    })
                    .catch((response)=>{
                        console.log(response);
                    });
            },
            go() {
                this.$http.post("http://111.198.146.40:3939/customize/Survey.asmx/GetAllQuestion", {}).then((response) => {
                    var d = JSON.parse(response.body.d);
                    this.questions = d.data;
                    this.questions.forEach(q=>{this.ans[q.QID]=[]});
                }, (response) => {
                    console.log(response);
                }).catch((response) => {
                    console.log(response);
                });
            },
            getpre() {
                this.$http.post("http://111.198.146.40:3939/customize/Survey.asmx/GetPreviousAnswer", {id:'12345619880808HZ10'}).then((response) => {
                    var d = JSON.parse(response.body.d);
                    if (d.status.code == 0) {
                        this.ans = d.data;
                        console.log(this.ans);
                        this.$forceUpdate();
                    }
                }, (response) => {
                    console.log(response);
                }).catch((response) => {
                    console.log(response);
                });
            },
            changed(qid, answer) {
                this.ans[qid] = answer;
                this.$forceUpdate();
            },
            getFirstNoe() {
                for (var i in this.questions) {
                    var q = this.questions[i];
                    if (this.noe(this.ans[q.QID]))
                        return q.QID;
                }
                return null;
            },
            nextstep() {
                var qid = this.getFirstNoe();
                if (qid === null) return;
                var elmnt = document.getElementById(qid);
                scrollElement(elmnt, 300);
                this.hint = true;
                this.$notify({
                  title: '提示',
                  message: '您有问题尚未回答，请完成红色标题的问题',
                  duration: 3000,
                  type: 'error'
                })
            }
        },
        components:{
            radios,
            checkboxs
        }
    }
</script>

<style>
    .el-checkbox{
        width:22%;
        margin-left:15px;
        display:inline-block;
        line-height:24px;
        color:#555;
    }
    .el-radio{
        margin-left:15px;
        color:#555;
    }
    .card {
        margin:0 auto;
        width:900px;
        padding-bottom:4px;
        border-radius: 10px;
        box-shadow: 2px 2px 2px #888;
        background-color: white;
    }
    .cardhead {
        width:100%;
        height:48px;
        line-height:48px;
        padding:0;
        color:white;
        background-color:rgb(100, 149, 237);
        border-radius:10px 10px 0 0;
    }
    .cardbody {
        padding: 16px 40px;
        margin-bottom: 16px;
        color: #555;
    }
    .ques {
        font-size: 1.2em;
        margin-top: 24px;
        margin-bottom: 6px;
    }
    .okcls {
        color:deepskyblue;
    }
    .errcls {
        color:red;
    }
    body {
        background-color: #f3f3f3;
    }
</style>