<template>
  <div id="app">
    <MyList :listData="nums" :itemSize="100"></MyList>
  </div>
</template>

<script>
import MyList from './components/myList.vue';

let aaa = [];
for(let i = 0; i<100; i++){
  let a = "数据: " + i;
  aaa.push({id: i, value: a})
}

export default {
  name: 'App',
  data() {
    return {
      nums: aaa
    }
  },
  components: {
    MyList
  },
}
</script>

<style>
html{
  height: 100%;
}
body{
  height: 100%;
  margin:0;
}
#app{
  height:100%;    /* app在html中的高度 */
  margin:0;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
