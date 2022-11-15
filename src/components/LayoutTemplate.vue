<template>
    <div>
        <v-navigation-drawer app v-model="drawer" width="16%" color="#93A9D1" hide-overlay>
            <v-list-item>
                <v-list-item-content>
                    <v-img :src="require('@/assets/vuefire.png')" contain max-height="60" style="-webkit-filter: drop-shadow( 2px 2px 2px #FFF);"></v-img>
                </v-list-item-content>
            </v-list-item>
            <v-divider></v-divider>
            <v-list nav>
                <v-list-item class="my-5" v-for="menu in menus" :key="menu.title" link tag="router-link" :to="menu.to">
                    <v-list-item-icon>
                        <v-icon color="#F7CACA">{{ menu.icon }}</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title style="font-family: Poppins; color: #F7CACA; font-size: 110.5%; font-weight: 600;">{{ menu.title}}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>
        <v-app-bar app fixed height="80%" color="#FFFF">
            <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
            <v-spacer></v-spacer>
            <span class="navbartext">
                <!-- XXXX: NPM -->
                Vuefire - 0860
            </span>
        </v-app-bar>
        <div style="min-height: 100vh !important;">
            <router-view></router-view>
        </div>
    </div>
</template>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Urbanist:wght@500&display=swap');

    .navbartext{
        font-family: Poppins; 
        font-style:bold; 
        color: black;
        font-size: 20px;
    }
    .textfield{
        font-family: Poppins !important;
        color: black;
    }
</style>

<script>
export default {
    name: 'LayoutTemplate',
    data () {
        return {
            drawer: true,
            menus: [
                {title: 'Merchandise', icon: 'mdi-hand-heart-outline', to:'/merchandise'},
                {title: 'Album', icon: 'mdi-album', to:'/album'},
            ],
        }
    }
}
</script>