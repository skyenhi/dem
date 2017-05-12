<template>
    <div class="main wrap clearfix">
        <div class="home-feeds cards-wrap">
            <div class="settings-main card">
                <div class="settings-main-content">
                    <a-input title="User name">
                        <input type="text" v-model="form.username" placeholder="username" class="base-input" name="username">
                        <span class="input-info error">Please enter username</span>
                    </a-input>
                    <a-input title="Password">
                        <input type="password" v-model="form.password" placeholder="password" class="base-input" name="password">
                        <span class="input-info error">Please enter password</span>
                    </a-input>
                </div>
                <div class="settings-footer clearfix">
                    <a @click="login" href="javascript:;" class="btn btn-yellow">Submit</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="babel">
import cookies from 'js-cookie'
import api from '~api'
import aInput from '~components/_input.vue'
export default {
    name: 'login',
    beforeRouteEnter (to, from, next) {
        if (cookies.get('b_user'))
            next('/backend/article/list')
        else
            next()
    },
    data() {
        return {
            form: {
                username: '',
                password: ''
            }
        }
    },
    components: {
        aInput
    },
    methods: {
        async login() {
            if (!this.form.username || !this.form.password) {
                this.$store.dispatch('global/showMsg', 'Please enter a user name and password!')
                return
            }
            const { data: { data, code} } = await api.post('backend/admin/login', this.form)
            if (data && code === 200) {
                this.$router.replace('/backend/article/list')
            }
        }
    }
}
</script>
