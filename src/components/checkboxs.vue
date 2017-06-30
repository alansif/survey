<template>
    <el-checkbox-group v-model="cursel" @change="change()">
        <el-checkbox v-for="item in items" :key="item.Aid" :label="item.Aid">{{item.Answer}}</el-checkbox>
    </el-checkbox-group>
</template>

<script>
    export default {
        props: ['items','qid','curselext'],
        data() {
            return {
                cursel: this.curselext || []
            }
        },
        watch:{
            curselext(v) {
                this.cursel = v || []
            }
        },
        methods:{
            change() {
                this.$emit('change', this.qid, this.cursel);
            }
        }
    }
</script>