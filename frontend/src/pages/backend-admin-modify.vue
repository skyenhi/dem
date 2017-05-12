<template>
    <div class="settings-main card">
        <div class="settings-main-content">
            <a-input title="Username">
                <input type="text" v-model="form.username" placeholder="username" class="base-input" name="username">
                <span class="input-info error">Please enter username</span>
            </a-input>
            <a-input title="Email">
                <input type="text" v-model="form.email" placeholder="email" class="base-input" name="email">
                <span class="input-info error">Please enter email</span>
            </a-input>
            <a-input title="Password">
                <input type="password" v-model="form.password" placeholder="password" class="base-input" name="password">
                <span class="input-info error">Please enter password</span>
            </a-input>
        </div>
        <div class="settings-footer clearfix">
            <router-link to="/backend/admin/list" class="btn btn-blue">Cancel</router-link>
            <a @click="modify" href="javascript:;" class="btn btn-yellow">Edit</a>
        </div>
    </div>
</template>

<script lang="babel">
import { mapGetters } from 'vuex'
import api from '~api'
import backendMenu from '~components/backend-menu.vue'
import aInput from '~components/_input.vue'
const fetchInitialData = async store => {
    await store.dispatch('backend/admin/getAdminItem')
}
export default {
    data() {
        return {
            form: {
                id: this.$route.params.id,
                username: '',
                email: '',
                password: ''
            }
        }
    },
    components: {
        aInput,
        backendMenu
    },
    computed: {
        ...mapGetters({
            item: 'backend/admin/getAdminItem'
        })
    },
    methods: {
        async modify() {
            if (!this.form.username || !this.form.email) {
                this.$store.dispatch('global/showMsg', 'Please complete the form!')
                return
            }
            const { data: { message, code, data} } = await api.post('backend/admin/modify', this.form)
            if (code === 200) {
                this.$store.dispatch('global/showMsg', {
                    type: 'success',
                    content: message
                })
                this.$store.commit('backend/admin/updateAdminItem', data)
                this.$router.push('/backend/admin/list')
            }
        }
    },
    mounted() {
        if (this.item.path !== this.$route.path) {
            fetchInitialData(this.$store)
        } else {
            this.form.username = this.item.data.username
            this.form.email = this.item.data.email
        }
    },
    watch: {
        item(val) {
            this.form.username = val.data.username
            this.form.email = val.data.email
        }
    }
}
</script>
