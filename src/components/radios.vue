<template>
    <el-radio-group v-model="cursel">
        <el-radio v-for="item in items" :key="item.Aid" :label="item.Aid">{{item.Answer}}</el-radio>
    </el-radio-group>
</template>

<script>
    export default {
        props: ['items'],
        data() {
            return {
                cursel: ''
            }
        }
    }
</script>