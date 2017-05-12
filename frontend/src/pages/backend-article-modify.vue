<template>
    <div class="settings-main card">
        <div class="settings-main-content">
            <a-input title="Title">
                <input type="text" v-model="form.title" placeholder="title" class="base-input" name="title">
                <span class="input-info error">Please enter the title</span>
            </a-input>
            <a-input title="Category" :classes="'select-item-wrap'">
                <i class="icon icon-arrow-down"></i>
                <select v-model="form.category" class="select-item" name="category">
                    <option value="">Please select a type</option>
                    <option v-for="item in category" :value="item._id">{{ item.cate_name }}</option>
                </select>
                <span class="input-info error">Please enter a category</span>
            </a-input>
            <div class="settings-section">
                <div id="modify-content" class="settings-item-content">
                    <textarea id="editor" name="content" class="form-control hidden" data-autosave="editor-content"></textarea>
                </div>
            </div>
        </div>
        <div class="settings-footer clearfix">
            <router-link to="/backend/article/list" class="btn btn-blue">Cancel</router-link>
            <a @click="modify" href="javascript:;" class="btn btn-yellow">Edit</a>
        </div>
    </div>
</template>

<script lang="babel">
/* global modifyEditor */
import api from '~api'
import { mapGetters } from 'vuex'
import aInput from '../components/_input.vue'
const fetchInitialData = async (store, config = { limit: 99}) => {
    await store.dispatch('global/category/getCategoryList', config)
}
export default {
    name: 'backend-article-modify',
    data() {
        return {
            form: {
                id: this.$route.params.id,
                title: '',
                category: '',
                category_name: '',
                category_old: '',
                content: ''
            }
        }
    },
    components: {
        aInput
    },
    computed: {
        ...mapGetters({
            category: 'global/category/getCategoryList'
        })
    },
    methods: {
        async modify() {
            const content = modifyEditor.getMarkdown()
            if (!this.form.title || !this.form.category || !content) {
                this.$store.dispatch('global/showMsg', 'Please complete the form!')
                return
            }
            this.form.content = content
            const { data: { message, code, data} } = await api.post('backend/article/modify', this.form)
            if (code === 200) {
                this.$store.dispatch('global/showMsg', {
                    type: 'success',
                    content: message
                })
                this.$store.commit('backend/article/updateArticleItem', data)
                this.$router.push('/backend/article/list')
            }
        }
    },
    mounted() {
        if (this.category.length <= 0) {
            fetchInitialData(this.$store)
        }
        this.$store.dispatch('backend/article/getArticleItem').then(data => {
            this.form.title = data.title
            this.form.category_old = data.category
            this.form.category = data.category
            this.form.content = data.content
            // eslint-disable-next-line
            window.modifyEditor = editormd("modify-content", {
                width: "100%",
                height: 500,
                markdown: data.content,
                placeholder: 'Please enter the content...',
                path: '/static/editor.md/lib/',
                toolbarIcons() {
                    return [
                        "bold", "italic", "quote", "|",
                        "list-ul", "list-ol", "hr", "|",
                        "link", "reference-link", "image", "code", "table", "|",
                        "watch", "preview", "fullscreen"
                    ]
                },
                watch : false,
                saveHTMLToTextarea : true
            })
        })
    },
    watch: {
        'form.category'(val) {
            const obj = this.category.find(item => item._id === val)
            this.form.category_name = obj.cate_name
        }
    }
}
</script>
