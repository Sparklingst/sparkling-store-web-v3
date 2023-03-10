<template>
    <div>

        <!-- 侧滑栏 -->
        <!-- 放到div里面屏蔽滑动 -->
        <div class="hidden-sm-and-down">
            <v-navigation-drawer v-model="drawer">
                <v-list lines="two">
                    <v-list-item :prepend-avatar="userStore.avatar" :title="userStore.nameShow"
                        :subtitle="userStore.email"
                        @click="userStore.loginState ? $router.push({ name: 'User' }) : userStore.login()" />
                </v-list>
                <v-divider></v-divider>
                <v-list density="compact" nav color="primary">
                    <v-list-item v-for="item in pages" :key="item.name" :to="{ name: item.name }" replace
                        :prepend-icon="$route.name == item.name ? item.activeIcon : item.icon" :title="item.title"
                        rounded />
                </v-list>
            </v-navigation-drawer>
        </div>
        <!-- 应用栏 density="comfortable" -->
        <v-app-bar flat border="b">
            <v-app-bar-nav-icon class="hidden-sm-and-down" @click.stop="drawer = !drawer" />
            <v-app-bar-title>{{ homeStore.fragmentTitle }}</v-app-bar-title>
            <v-spacer />
            <v-tooltip location="bottom" transition="fade-transition">
                <template v-slot:activator="{ props }">
                    <v-btn icon="mdi-magnify" v-bind="props" />
                </template>
                <span>搜索应用</span>
            </v-tooltip>

            <v-menu :width="196" origin="overlap">
                <template v-slot:activator="{ props: menu }">
                    <v-btn icon="mdi-dots-vertical" v-bind="menu" />
                </template>
                <v-list>
                    <v-list-item title="上传应用" :to="{ name: 'Upload' }" />
                    <v-list-item link :title="userStore.loginState ? '退出登录' : '登录'"
                        @click="userStore.loginState ? userStore.logout() : userStore.login()" />
                    <v-list-item v-if="installBtnVisible" title="安装应用" @click="onInstallBtnClick" />
                </v-list>
            </v-menu>

        </v-app-bar>

        <!-- 主视图 -->
        <AppMain class="main">
            <router-view v-slot="{ Component }">
                <keep-alive>
                    <component :is="Component" />
                </keep-alive>
            </router-view>
        </AppMain>

        <!-- 底部导航栏 -->
        <v-bottom-navigation grow class="hidden-md-and-up" color="primary" mandatory border="t" :elevation="0">
            <v-btn variant="plain" v-for="item in pages" :key="item.name" :to="{ name: item.name }" replace>
                <v-icon>{{ $route.name == item.name ? item.activeIcon : item.icon }}</v-icon>
                {{ item.title }}

            </v-btn>
        </v-bottom-navigation>
    </div>
</template>

<script setup>
import { ref, inject } from "vue";
import { useUserStore } from '@/store/user';
import { useHomeStore } from '@/store/home';

const userStore = useUserStore()
const homeStore = useHomeStore()

const drawer = ref(null)
const pages = [
    {
        title: "首页",
        icon: "mdi-home-variant-outline",
        activeIcon: "mdi-home-variant",
        name: "Home",
    },
    {
        title: "分类",
        icon: "mdi-apps",
        activeIcon: "mdi-apps",
        name: "Apps",
    },
    {
        title: "更新",
        icon: "mdi-update",
        activeIcon: "mdi-update",
        name: "Update",
    },
    {
        title: "我的",
        icon: "mdi-account-outline",
        activeIcon: "mdi-account",
        name: "Me",
    },
]

const installBtnVisible = inject('installBtnVisible')
const onInstallBtnClick = inject('onInstallBtnClick')

</script>

<style scoped>
@media (min-width: 960px) {
    .main {
        --v-layout-left: 256px;
        --v-layout-bottom: 0 !important;
    }
}

@media (max-width: 959.98px) {
    .main {
        --v-layout-left: 0 !important;
    }
}

@media (min-width: 960px) {
    :deep(.v-container) {
        max-width: 900px !important;
    }
}
</style>