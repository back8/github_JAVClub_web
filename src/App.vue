<template>
    <a-layout id="components-layout-top" class="layout">
        <a-layout-header>
        <a-menu
        theme="dark"
        mode="horizontal"
        :defaultSelectedKeys="[routeName]"
        :style="{ lineHeight: '64px' }"
        >
        <template v-if="isLogin">
            <a-menu-item key="homepage">
                <a-icon type="home" />
                <span>Homepage</span>
                <router-link :to="{ name: 'homepage' }" />
            </a-menu-item>

            <a-sub-menu>
                <span slot="title" class="submenu-title-wrapper">
                    <a-icon type="switcher" />Metas
                </span>

                <a-menu-item key="metadataList">
                    <a-icon type="menu" />
                    <span>Metadatas</span>
                    <router-link :to="{ name: 'metadataList' }" />
                </a-menu-item>

                <a-menu-item key="tagList">
                    <a-icon type="tags" />
                    <span>Tags</span>
                    <router-link :to="{ name: 'tagList' }" />
                </a-menu-item>

                <a-menu-item key="starList">
                    <a-icon type="team" />
                    <span>Stars</span>
                    <router-link :to="{ name: 'starList' }" />
                </a-menu-item>

                <a-menu-item key="seriesList">
                    <a-icon type="number" />
                    <span>Series</span>
                    <router-link :to="{ name: 'seriesList' }" />
                </a-menu-item>
            </a-sub-menu>

            <a-menu-item key="bookmarkList">
                <a-icon type="folder" />
                <span>Bookmarks</span>
                <router-link :to="{ name: 'bookmarkList' }" />
            </a-menu-item>
            
            <a-menu-item key="profile">
                <a-icon type="user" />
                <span>Profile</span>
                <router-link :to="{ name: 'profile' }" />
            </a-menu-item>

            <a-menu-item key="admin" v-if="isAdmin">
                <a-icon type="solution" />
                <span>Admin</span>
                <router-link :to="{ name: 'admin' }" />
            </a-menu-item>

            <a-menu-item key="logout" v-on:click="logout">
                <a-icon type="logout" />
                <span>Logout</span>
            </a-menu-item>
        </template>
        <template v-else>
            <a-menu-item key="login">
                <a-icon type="login" />
                <span>Login</span>
                <router-link :to="{ name: 'login' }" />
            </a-menu-item>
            <a-menu-item key="signup">
                <a-icon type="user-add" />
                <span>Sign Up</span>
                <router-link :to="{ name: 'signup' }" />
            </a-menu-item>
        </template>
        </a-menu>
        </a-layout-header>

        <a-layout-content style="padding: 0 25px; align-items: center; text-align: center;">
            <div :style="{ background: '#fff', padding: '24px' }">
                <router-view></router-view>
            </div>
        </a-layout-content>
    </a-layout>
</template>
<script>
    export default {
        data: function() {
            return {
                collapsed: false,
                isLogin: false,
                isAdmin: false
            };
        },

        computed: {
            routeName: function() {
                return this.$route.name || 'homepage'
            }
        },

        methods: {
            logout: function() {
                this.axios.get(this.apiHost + '/auth/logout')
                this.isLogin = false
                window.location.reload()
            }
        },

        created: function() {
            this.axios.get(this.apiHost + '/auth/check').then((res) => {
                res = res.data

                this.isLogin = res.data.isLogin
                this.isAdmin = (this.isLogin) ? res.data.permission.rule.admin : false
                if (!this.isLogin) {
                    console.log(this.$route.name)
                    if (this.$route.name !== 'login') {
                        this.$router.push({ name: 'login' })
                    }
                    return
                }

                if (!this.$route.name) {
                    this.$router.push({ name: 'homepage' })
                    return
                }
            })
        }
    };
</script>
<style>

</style>