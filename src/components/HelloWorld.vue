<script setup lang="ts">
import { ref } from 'vue'

const UserId = ref('')
const Cookie = ref('')
const Url = ref('')
const dialogShow = ref(false);
const msg = ref('');
function showDialog(msgarg:string){
  msg.value = msgarg;
  dialogShow.value = true;
  setTimeout(() => {
    dialogShow.value = false;
  }, 2000);
}
function start() {
  // post to http://43.163.218.46:8081/user/lottery/temp
  // body: {"userId":"123","cookie":"123","url":"123"}
  if(!UserId.value || !Cookie.value || !Url.value) {
    showDialog('请填写完整信息')
    return
  }
  fetch('http://43.163.218.46:8081/user/lottery/temp',
    {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        userId: UserId.value,
        cookie: Cookie.value,
        url: Url.value
      })
    }).then(res => res.json()).then(res => {
      if(res.code === 200) {
        showDialog('抽奖成功')
      } else {
        showDialog(res.msg)
      }
    }, err => {
      showDialog('抽奖失败')
    })
}
</script>

<template>
  <div class="form">
    <div class="form-group">
      <label for="name">用户ID</label>
      <input type="text" class="form-control" id="name" placeholder="请输入" v-model="UserId">
    </div>
    <div class="form-group">
      <label for="name">Cookie</label>
      <input type="text" class="form-control" id="name" placeholder="请输入" v-model="Cookie">
    </div>
    <div class="form-group">
      <label for="name">解析动态URL</label>
      <input type="text" class="form-control" id="name" placeholder="请输入" v-model="Url">
    </div>
    <button type="submit" @click="start" class="btn btn-primary">开始抽奖</button>
  </div>
  <Transition name="fade">
    <div v-if="dialogShow" class="dialog" >
      {{ msg  }}
    </div>
  </Transition>

</template>

<style scoped>
.form {
  width: 300px;
  margin: 0 auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0,0,0,.2);

}

.form-group {
  margin-bottom: 1rem;
  display: flex;
}
.form-group label {
  width: 100px;
  margin-right: 1rem;
  text-align: right;
}


.dialog{
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  padding: 1rem;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0,0,0,.2);
  z-index: 999;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0
}
</style>
