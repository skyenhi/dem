<template>
    <nav class="global-nav">
        <div v-if="backend" class="wrap clearfix">
            <div class="left-part">
                <div class="main-nav">
                    <a href="/" class="nav-link"><i class="icon icon-nav-home"></i><span class="text">Home</span></a>
                </div>
            </div>
        </div>
        <div v-else class="wrap clearfix">
            <div class="left-part">
                <div class="main-nav">
                    <router-link to="/" active-class="current" exact class="nav-link"><i class="icon icon-nav-home"></i><span class="text">Home</span></router-link>
                </div>
            </div>
            <div class="right-part">
                <span v-if="isLogin" class="nav-me"><router-link to="/user/account" class="nav-me-link"><img src="/static/images/avatar.png" class="nav-avatar-img"></router-link></span>
                <span v-else class="nav-me"><a @click="login" href="javascript:;" class="nav-me-link"><img src="/static/images/user.png" class="nav-avatar-img"></a></span>
            </div>
        </div>
    </nav>
</template>

<script lang="babel">
import cookies from 'js-cookie'
export default {
    props: ['backend'],
    data() {
        return {
            isLogin: cookies.get('user')
        }
    },
    methods: {
        login() {
            this.$store.commit('global/showLoginModal', true)
        }
    }
}
</script>
