<template>
    <el-radio-group v-model="cursel" @change="change()">
        <el-radio v-for="item in items" :key="item.Aid" :label="item.Aid">{{item.Answer}}</el-radio>
    </el-radio-group>
</template>

<script>
    export default {
        props: ['items','qid','curselext'],
        data() {
            return {
                cursel: (this.curselext && this.curselext[0]) || ""
            }
        },
        watch:{
            curselext(v) {
                this.cursel = (v && v[0]) || ""
            }
        },
        methods:{
            change() {
                this.$emit('change', this.qid, this.cursel.length === 0 ? [] : [this.cursel]);
            }
        }
    }
</script>