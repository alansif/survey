<template>
    <el-checkbox-group v-model="cursel" @change="change()">
        <el-checkbox v-for="item in items" :key="item.Aid" :label="item.Aid">{{item.Answer}}</el-checkbox>
    </el-checkbox-group>
</template>

<script>
    export default {
        props: ['items','qid'],
        data() {
            return {
                cursel: []
            }
        },
        methods:{
            change() {
                this.$emit('change', this.qid, this.cursel);
            }
        }
    }
</script>