<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-sm-4 mt-5">
                <form v-if="isAuthenticated == false" @submit.prevent="login" >
                    <div class="form-group">
                        <label for="exampleInputEmail1">Email address</label>
                        <input type="email" class="form-control" v-model="form.email"  placeholder="Enter email">
                        
                    </div>
                    <div class="form-group mt-2">
                        <label for="exampleInputPassword1">Password</label>
                        <input type="password" class="form-control" v-model="form.password" placeholder="Password">
                    </div>
                    <button type="submit" class="btn btn-primary mt-2">Submit</button>
                </form>
                <div v-else>
                    <h1>Dashboard...</h1>
                    <button class="btn btn-dark" @click="logout">Logout</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, inject, ref, onMounted } from 'vue';
import axios from 'axios'

export default {
    setup(){
        let cookies = inject('cookies')
        let isAuthenticated = ref(false)
        const form = reactive({
            email: 'cseazizul@gmail.com',
            password: 'pasword'
        });
        
        const login = async() => {
            let res = await axios.post('api/login', form)
            if(res.data.token) {
                cookies.set('access_token',res.data.token)
                isAuthenticated.value = true
            }
            console.log(cookies.get('access_token'))
        }
        const checkLogin = ()=>{
            if(cookies.get('access_token')) {
                isAuthenticated.value = true
            }
         }
         const logout = ()=>{
            if(cookies.get('access_token')) {
                cookies.set('access_token','')
                isAuthenticated.value = false
            }
         }

        onMounted(checkLogin)

        return {
            form,
            login,
            isAuthenticated,
            logout
        }
    }
}
</script>