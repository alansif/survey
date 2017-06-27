<template>
    <el-checkbox-group v-model="cursel">
        <el-checkbox v-for="item in items" :key="item.Aid" :label="item.Aid">{{item.Answer}}</el-checkbox>
    </el-checkbox-group>
</template>

<script>
    export default {
        props: ['items'],
        data() {
            return {
                cursel: []
            }
        }
    }
</script>