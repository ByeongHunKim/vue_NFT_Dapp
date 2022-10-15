<template>
  <div>
    <!-- vue에서 if문 사용 : 메타마스크가 설치되었으면 아래 문구 렌더링, 미설치 시 v-else 가 적용되어있는 div태그 렌더링 -->
    <!-- v-if, else는 꼭 붙어있어야 한다. -->
    <div v-if="isInstallMetaMask">
      메타마스크 설치됨
    </div>
    <div v-else>
      메타마스크 설치되지 않음
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      // 이 변수에는 뒤의 조건이 모두 true일 때 true가 저장된다.
      isInstallMetaMask : window.ethereum.isMetaMask != null && window.ethereum.isMetaMask == true
    }
  },
  methods:{

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
