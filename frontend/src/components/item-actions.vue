<template>
    <div class="actions-wrap">
        <a @click="like" href="javascript:;" class="action-item" :class="item.like_status ? 'active' : ''"><i class="icon" :class="item.like_status ? 'icon-action-voteup-active' : 'icon-action-voteup'"></i><span class="text">{{ item.like }} Like</span></a>
        <a href="javascript:;" class="action-item"><i class="icon icon-action-comment"></i><span class="text">{{ item.comment_count }} Comment</span></a>
    </div>
</template>
<script lang="babel">
import cookies from 'js-cookie'
import api from '~api'
export default {
    name: 'item-actions',
    serverCacheKey: props => {
        return `frontend::topics::item::actions::${props.item._id}`
    },
    props: ['item'],
    methods: {
        async like() {
            const username = cookies.get('user')
            if (!username) {
                this.$store.dispatch('global/showMsg', 'please log in first!')
                this.$store.commit('global/showLoginModal', true)
                return
            }
            let url = 'frontend/like'
            if (this.item.like_status) url = 'frontend/unlike'
            const { data: {code, message} } = await api.get(url, { id: this.item._id })
            if (code === 200) {
                this.$store.commit('frontend/article/modifyLikeStatus', {id: this.item._id, status: !this.item.like_status})
                this.$store.dispatch('global/showMsg', {
                    content: message,
                    type: 'success'
                })
            }
        }
    }
}
</script>
