<template>
  <div>
    <!-- vue에서 if문 사용 : 메타마스크가 설치되었으면 아래 문구 렌더링, 미설치 시 v-else 가 적용되어있는 div태그 렌더링 -->
    <!-- v-if, else는 꼭 붙어있어야 한다. -->
    <div v-if="isInstallMetaMask">
      <div>메타마스크 설치됨</div>
      <div v-if="myAddress == null">
        <button v-on:click="connectWallet">메타마스크 지갑 연결</button>
      </div>

      <div v-else>
        <!-- 지갑이 연결이 된 이후 -->
        <div v-if="myContract == null">
          <button v-on:click="connectToContract">컨트랙트 연결</button>
        </div>
        <div v-else>
          <!-- 컨트랙트까지 연결된 이후  -->
          <div> <!-- ownerOf 구현 -->
            <input type="text" placeholder="tokenId" v-model="ownerOf_tokenId">
            <button v-on:click="requestOwnerOf">ownerOf 요청</button>
            <div>
              <!-- 결과값 -->
              {{ownerOf_result}}
            </div>
          </div>
          <div><!-- balanceOf 구현 -->
            <input type="text" placeholder="address" v-model="balnceOf_address">
            <button v-on:click="requestBalanceOf">balanceOf 요청</button>
            <div>
              {{balanceOf_result}}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <div>메타마스크 설치되지 않음</div>
    </div>
  </div>
</template>

<script>
import {ethers} from "ethers";
import abi from "../assets/abi.json";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      // 이 변수에는 뒤의 조건이 모두 true일 때 true가 저장된다.
      isInstallMetaMask : window.ethereum.isMetaMask != null && window.ethereum.isMetaMask == true,
      myAddress : null,
      myContract : null,
      // input field에 넣은 값이 이 변수에 저장
      ownerOf_tokenId : null,
      ownerOf_result : null,
      balnceOf_address : null,
      balanceOf_result : null,
    }
  },
  methods:{
    async connectWallet()
    { 
      console.log("click : connectWallet");
      var addressArray = await window.ethereum.request({method : "eth_requestAccounts"});
      // 연결된 지갑의 주소값이 배열에 들어있다.
      if ( addressArray.length > 0)
      {
        this.myAddress = addressArray[0];
      }
      console.log(addressArray);
    },
    connectToContract()
    {
      console.log("click : connect to contract");
      var contractAddress = "0x60aCf6de68fEd0c2608e9DC96077f51786c13dda";
      // 객체들을 동적 할당할 때 new를 사용한다.
      var provider = new ethers.providers.Web3Provider(window.ethereum);
      var signer = provider.getSigner();
      this.myContract = new ethers.Contract(contractAddress, abi, signer);
      console.log(this.myContract);
    },
    async requestOwnerOf()
    {
      console.log("click requestOwnerOf");
      console.log(this.ownerOf_tokenId);
      this.ownerOf_result = await this.myContract.ownerOf(this.ownerOf_tokenId)
      console.log(this.ownerOf_result);
    },
    async requestBalanceOf()
    {
      console.log("click requestBalanceOf");
      console.log(this.balnceOf_address);
      this.balanceOf_result = await this.myContract.balanceOf(this.balnceOf_address)
      console.log(this.balanceOf_result);
    }
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
