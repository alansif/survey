<template>
    <el-radio-group v-model="cursel" @change="change()">
        <el-radio v-for="item in items" :key="item.Aid" :label="item.Aid">{{item.Answer}}</el-radio>
    </el-radio-group>
</template>

<script>
    export default {
        props: ['items','qid'],
        data() {
            return {
                cursel: ''
            }
        },
        methods:{
            change() {
                this.$emit('change', this.qid, this.cursel);
            }
        }
    }
</script>