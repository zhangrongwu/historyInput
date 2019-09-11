<template>
  <div class="hello">
    <historyInput label="备注：" placeholder="请输入备注内容" v-model="msg" historyKey="001010" :maxHistory="10"></historyInput>
    <historyInput label="张荣武：" placeholder="请输入张荣武内容" v-model="name" historyKey="001011" :maxHistory="4"></historyInput>
    <historyInput label="开发人员：" placeholder="请输入开发人员内容" v-model="msg" historyKey="001012" :maxHistory="2"></historyInput>
    <historyInput label="QQ：" placeholder="请输入QQ" v-model="QQ" historyKey="001014" :maxHistory="8"></historyInput>
  </div>
</template>

<script>
import historyInput from "./historyInput"
export default {
  components: {
    historyInput,
  },
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      QQ: "3153256054",
      name: "张荣武"
    }
  }
}
</script>

<style scoped>
</style>
