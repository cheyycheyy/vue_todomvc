<template>
    <div id="Frame">
        <!-- 动态组件 -->
        <div :is='contant'/>
        <div :is='toDoFooter'/>
    </div>
</template>

<script>
// 导入组件
import Contant from '@/components/Contant'
import ToDoFooter from '@/components/ToDoFooter'


export default {
    name: 'Frame',
    data() {
        return {
            // 挂在组件
            contant: 'Contant',
            toDoFooter: 'ToDoFooter',
        }
    },
    // 注册组件
    components: {
        Contant,
        ToDoFooter,
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
