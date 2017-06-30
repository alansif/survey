<template>
    <div class="card">
        <el-button type="primary" @click="go()">click</el-button>
        <el-button type="primary" @click="getpre()">GetPreviousAnswer</el-button>
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
                questions: [],
                ans:{},
                hint:false
            }
        },
        methods: {
            noe(a) {
                return !a || (a.length === 0);
            },
            go() {
                this.$http.post("http://111.198.146.40:8082/customize/Survey.asmx/GetAllQuestion", {}).then((response) => {
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
                this.$http.post("http://111.198.146.40:8082/customize/Survey.asmx/GetPreviousAnswer", {id:'12345619880808HZ10'}).then((response) => {
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
//                console.log(document.documentElement.scrollTop);
//                console.log(document.body.scrollTop);
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
        padding: 32px 40px;
        margin-bottom: 16px;
        border: 1px solid #ddd;
        border-radius: 10px;
        color: #555;
        background-color: #f7f7f7;
        box-shadow: 1px 1px 1px #888;
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
</style>