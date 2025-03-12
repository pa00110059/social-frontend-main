<template>
    <div class="login-container">
        <el-form
            :model="loginForm"
            ref="formRef"
            label-width="80px"
            class="login-form"
            :rules="loginRules"
        >
            <h2>用戶登錄</h2>
            <el-form-item label="用戶名" prop="username">
                <el-input
                    v-model="loginForm.username"
                    ref="username"
                    name="username"
                    autocomplete="off"
                    placeholder="請輸入用戶名"
                ></el-input>
            </el-form-item>
            <el-form-item label="密碼" prop="userPwd">
                <el-input
                    type="password"
                    v-model="loginForm.userPwd"
                    autocomplete="off"
                    placeholder="請輸入密碼"
                ></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="success" @click.native.prevent="login">登入</el-button>
                <el-button type="primary" @click="toRegister">註冊</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
    name: "Login",
});
</script>
<script lang="ts" setup>
import { ref } from "vue";
import { useUserInfoStore } from "../../stores/userInfo";

import type { FormInstance } from "element-plus/es/components/form";
import { useRouter } from "vue-router";
const userInfoStore = useUserInfoStore();
const router = useRouter();
const formRef = ref<FormInstance>();
const loading = ref(false);
//账号密码参数
const loginForm = ref({
    username: "zhangsan",
    userPwd: "123456",
});
// 校验规则
const validateUsername = (rule: any, value: any, callback: any) => {
    if (value.length < 4) {
        callback(new Error("用戶名長度不能少於4位"));
    } else {
        callback();
    }
};
// 校验规则
const validatePassword = (rule: any, value: any, callback: any) => {
    if (value.length < 6) {
        callback(new Error("密碼長度不能少於6位"));
    } else {
        callback();
    }
};
// 校验规则
const loginRules = {
    username: [{ required: true, validator: validateUsername }],
    userPwd: [{ required: true, trigger: "blur", validator: validatePassword }],
};
//点击登录的回调
const login = async () => {
    // console.log('点击登录');
    await formRef.value?.validate();
    loading.value = true;
    try {
        // await getUserInfo(loginForm.value)
        await userInfoStore.login(loginForm.value);
        router.push({ name: "HeadlineNews" });
    } finally {
        loading.value = false;
    }
    // loading.value = true
    // const { username, userPwd } = loginForm.value
    // try {
    //   await userInfoStore.login(username, userPwd)
    //   router.push({ path: redirect.value || '/' })
    // } finally {
    //   loading.value = false
    // }
};

const toRegister = () => {
    router.push({ name: "Register" });
};
</script>

<style scoped>
.login-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
.login-form {
    width: 400px;
    text-align: center;
}
</style>
