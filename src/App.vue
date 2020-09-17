<template>
  <div id="app">
    <div class='find'>
      <b-form-input type="text" name="" id="" 
        placeholder='IP'
        class='mb-3 ip-input'
        v-model='firstIP'
        :state='isValid(firstIP)'
      ></b-form-input>
      <b-form-input type="text" name="" id="" 
        placeholder='До'
        class='mb-3 ip-input'
        v-model='secondIP'
        :state='isValid(secondIP)'
      ></b-form-input>
      <b-button 
        variant="success"
        size="lg"
        @click='getListIp(firstIP,secondIP)'
        class="mb-3"
      >Найти диапазон</b-button>
      <b-alert show variant="danger"
        v-if='fail || fail_last'
        class='surfacing'>Неверный диапазон!</b-alert>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstIP: '',
      secondIP: '',
      fail: false,
      fail_last: false,
      regexp : /^(25[0-5]|2[0-4][0-9]|[0-1][0-9]{2}|[0-9]{2}|[0-9])(\.(25[0-5]|2[0-4][0-9]|[0-1][0-9]{2}|[0-9]{2}|[0-9])){3}$/,
      resJSON: '',
    }
  },
  methods: {
    getListIp(ip1, ip2) {
      let resArr = []
      let res = []
      let ipArr1 = ip1.split('.').map(e => +e)
      let ipArr2 = ip2.split('.').map(e => +e)      
      for (let i = 0; i <= 2; i++) {
        if (ipArr1[i] != ipArr2[i] || ipArr1.length != 4) {
          this.fail = true
          break;
        } else {
          this.fail = false
          res.push(ipArr1[i])
        }
      }
      if (ipArr1[3] >= ipArr2[3]) {
        this.fail_last = true
      } else {
        this.fail_last = false
        res.push(ipArr1[3])
      }

      for (let i = ipArr1[3]; i <= ipArr2[3]; i++) {
        res[3] = i
        resArr.push(res.join('.'))
      }

      if (ip1.length == 0 || ip2.length == 0) {
        this.fail = true
      }

      if (!this.fail && !this.fail_last) {
        // let resJSON = JSON.stringify(resArr)
        console.log(resArr)
      }


    },
    isValid(ip) {
      if (ip == '') {
        return
      } else {
        return ip.match(this.regexp) ? true : false
      }
    },
  },
  computed: {

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.find {
  border: 1px solid #000;
  border-radius: 5px;
  width: 300px;
  padding: 25px;
  margin: 0 auto 10px;
}

.surfacing {
  animation: bounceIn 0.3s;
}

@keyframes bounceIn {
  0% {
    transform: scale(0.1);
    opacity: 0;
  }
  60% {
    transform: scale(1.2);
    opacity: 1;
  }
  100% {
    transform: scale(1);
  }
}



</style>
