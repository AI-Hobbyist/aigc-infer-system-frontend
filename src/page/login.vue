<template>
    <n-layout-content bordered class="layout-content" style="padding: 800px;padding-top: 50px;background: rgba(240, 255, 255, 0);">
    <div class="box">
        <!-- <div"> -->
        <n-space vertical class="input">
            <div class="title">登录</div>
            <n-input v-model:value="email" round  type="email" placeholder="邮箱" />
            <n-input v-model:value="pwd" round  type="password" placeholder="密码" />
            <n-button type="primary" round style="width: 100%;" @click="login">登录</n-button>
        </n-space>
        <n-a href="https://reg.ai-hobbyist.org">点击注册账号</n-a>
    </div>
</n-layout-content>
</template>
<script lang="ts" setup>
import { ref } from 'vue';
import user from '../api/user'
import { useUserStore } from '../store';
import { useNotification } from 'naive-ui'
import { useRouter } from 'vue-router';
const notification = useNotification()
const pwd = ref("")
const email = ref("")
const router = useRouter()
const store = useUserStore()
const login = async () => {
    try {
        const data = await user.login({lang: "zh",email: email.value,password: pwd.value,mac: ""})
        if(data.token) {
            store.token.value = data.token
            notification.success({
                content: data.message.replace(/(?:\\r\\n|\\r|\\n)/g, '\n'),
                duration: 5000
            })
            router.push("/infer")
        } else {
            notification.error({
                content: data.message,
                duration: 3000
            })
        }
    } catch(e) {
        notification.error({
            content: "API 调用失败，请联系网站维护者",
            duration: 3000
        })
        console.error(e)
    }
}

</script>
<style scoped>
@media screen and (max-width: 768px) {
  .layout-content {
    padding: 50px !important ;
  }
}
@media screen and (min-width: 768px) {
  .layout-content {
    padding: 0 40vw !important ;
  }
}
.box {
    width: 100%;
    background-color: rgba(240, 248, 255, 0.329);
    backdrop-filter: blur(5px);
    display: flex;
    margin-top: 50px;
    /* align-content: center; */
    align-items: center;
    flex-direction: column; 
    border-radius: 17px; 
    padding: 20px 0px
}
.title {
    font-size: 24px;
    font-weight: 600;
    text-align: center;
}
.input {
    width: 80%;
}
</style>