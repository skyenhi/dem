<template>
    <div class="main wrap clearfix">
        <div class="main-left">
            <div class="home-feeds cards-wrap">
                <div class="settings-main card">
                    <div class="settings-main-content">
                        <a-input title="Current password">
                            <input type="password" v-model="form.old_password" placeholder="Current Password" class="base-input" name="old_password">
                        </a-input>
                        <a-input title="Password">
                            <input type="password" v-model="form.password" placeholder="Password" class="base-input" name="password">
                        </a-input>
                        <a-input title="Confirm password">
                            <input type="password" v-model="form.re_password" placeholder="Confirm password" class="base-input" name="re_password">
                        </a-input>
                    </div>
                    <div class="settings-footer clearfix">
                        <a @click="modify" href="javascript:;" class="btn btn-yellow">Save</a>
                    </div>
                </div>
            </div>
        </div>
        <div class="main-right">
            <account></account>
        </div>
    </div>
</template>

<script lang="babel">
import api from '~api'
import account from '~components/aside-account.vue'
import aInput from '~components/_input.vue'
export default {
    data() {
        return {
            form: {
                old_password: '',
                password: '',
                re_password: ''
            }
        }
    },
    components: {
        aInput,
        account
    },
    methods: {
        async modify() {
            if (!this.form.password || !this.form.old_password || !this.form.re_password) {
                this.$store.dispatch('global/showMsg', 'Please complete the form!')
                return
            } else if (this.form.password !== this.form.re_password) {
                this.$store.dispatch('global/showMsg', 'Two password entries are inconsistent!')
                return
            }
            const { data: { code, data} } = await api.post('frontend/user/password', this.form)
            if (code === 200) {
                this.$store.dispatch('global/showMsg', {
                    type: 'success',
                    content: data
                })
                this.form.old_password = ''
                this.form.password = ''
                this.form.re_password = ''
            }
        }
    },
    mounted() {
        this.$store.dispatch('global/gProgress', 100)
    },
    metaInfo () {
        return {
            title: 'Demo Blog',
            meta: [{ vmid: 'description', name: 'description', content: 'Blog with Vue' }]
        }
    }
}
</script>
