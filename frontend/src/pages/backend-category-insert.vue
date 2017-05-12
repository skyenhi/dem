<template>
    <div class="settings-main card">
        <div class="settings-main-content">
            <a-input title="Category">
                <input type="text" v-model="form.cate_name" placeholder="category" class="base-input" name="cate_name">
                <span class="input-info error">Please enter a category name</span>
            </a-input>
            <a-input title="Category order">
                <input type="text" v-model="form.cate_order" placeholder="Category order" class="base-input" name="cate_order">
                <span class="input-info error">Please enter a category order</span>
            </a-input>
        </div>
        <div class="settings-footer clearfix">
            <a @click="insert" href="javascript:;" class="btn btn-yellow">Add</a>
        </div>
    </div>
</template>

<script lang="babel">
import api from '~api'
import aInput from '../components/_input.vue'
export default {
    name: 'backend-category-insert',
    data() {
        return {
            form: {
                cate_name: '',
                cate_order: ''
            }
        }
    },
    components: {
        aInput
    },
    methods: {
        async insert() {
            if (!this.form.cate_name || !this.form.cate_order) {
                this.$store.dispatch('global/showMsg', 'Please complete the form!')
                return
            }
            const { data: { message, code, data} } = await api.post('backend/category/insert', this.form)
            if (code === 200) {
                this.$store.dispatch('global/showMsg', {
                    type: 'success',
                    content: message
                })
                this.$store.commit('global/category/insertCategoryItem', {
                    ...this.form,
                    _id: data
                })
                this.$router.push('/backend/category/list')
            }
        }
    }
}
</script>
