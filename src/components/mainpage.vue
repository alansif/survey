<template>
    <div class="card">
        <el-button type="primary" @click="go()">click</el-button>
        <template v-for="(question,idx) in questions">
            <p class="ques">{{question.QID}}.{{question.Question}}</p>
            <checkboxs v-if="question.Multiple" :items="question.Alts"></checkboxs>
            <radios v-else :items="question.Alts"></radios>
        </template>
    </div>
</template>

<script>
    import radios from './radios.vue'
    import checkboxs from './checkboxs.vue'
    export default {
        data () {
            return {
                questions: [],
                answers:{'M1':'B'}
            }
        },
        methods: {
            go() {
                this.$http.post("http://111.198.146.40:8082/customize/Survey.asmx/GetAllQuestion", {}).then((response) => {
                    var d = JSON.parse(response.body.d);
                    this.questions = d.data;
                    this.questions.forEach(q=>{
                        this.answers[q.QID] = "B"
                    });
                    console.log(this.questions);
                }, (response) => {
                    console.log(response);
                }).catch((response) => {
                    console.log(response);
                });
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
        background-color: #f3f3f3;
        box-shadow: 1px 1px 1px #888;
    }
    .ques {
        font-size: 1.2em;
        color:deepskyblue;
        margin-top: 24px;
        margin-bottom: 6px;
    }
</style>