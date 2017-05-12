<template>
    <div class="main-right">
        <div class="card card-me">
            <router-link to="/backend/admin/list" active-class="active" class="side-entry"><i class="icon icon-arrow-right"></i><i class="icon icon-articles"></i>Manage account number</router-link>
            <router-link to="/backend/user/list" active-class="active" class="side-entry"><i class="icon icon-arrow-right"></i><i class="icon icon-articles"></i>User list</router-link>
        </div>
        <div class="card card-me">
            <router-link to="/backend/category/insert" active-class="active" class="side-entry"><i class="icon icon-arrow-right"></i><i class="icon icon-articles"></i>add category</router-link>
            <router-link to="/backend/category/list" active-class="active" class="side-entry"><i class="icon icon-arrow-right"></i><i class="icon icon-articles"></i>Management classification</router-link>
            <router-link to="/backend/article/insert" active-class="active" class="side-entry"><i class="icon icon-arrow-right"></i><i class="icon icon-articles"></i>post article</router-link>
            <router-link to="/backend/article/list" active-class="active" class="side-entry"><i class="icon icon-arrow-right"></i><i class="icon icon-articles"></i>Manage articles</router-link>
        </div>
    </div>
</template>

<script lang="babel">
export default {
    name: 'backend-menu',
    serverCacheKey: () => {
        return `backend::menu`
    }
}
</script>
