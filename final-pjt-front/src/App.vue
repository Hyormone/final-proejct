<template>
  <div id="app">
    <div :class="sidebarStatus ? 'sidebar-opened sidebar' : 'sidebar-closed sidebar'" @mouseleave="closeNav">
      <p class="closebtn" @click="closeNav">&times;</p>
      <v-sheet
        color="rgb(17, 17, 17)"
        class="pa-4"
      >
        <v-avatar
          class="mb-4"
          color="grey darken-1"
          size="64"
        ></v-avatar>

        <!-- <div style="color: white">{{ currUser?.username }}님의 프로필</div> -->
      </v-sheet>

      <v-divider></v-divider>

      <v-list style="background-color: rgb(17, 17, 17)">
        <v-list-item
          v-for="[icon, text] in links"
          :key="icon"
          link
        >
          <v-list-item-icon dark>
            <v-icon style="color: white">{{ icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content dark>
            <v-list-item-title style="color: white">{{ text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </div>
    
    <nav :class="[navBarStatus ? 'scroll-down' : '', navbar, navbar-expand, ps-4, pe-3, pt-4, pb-4, d-flex, justify-content-between]">
    <!-- <nav class="navbar navbar-expand ps-4 pe-3 pt-4 pb-4 d-flex justify-content-between"> -->
      <div class="logo" @click="reload">
        <router-link :to="{ name: 'MainView' }">
          <span>
            neon lights
          </span>
        </router-link>
      </div>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <div class="search">
          <input id="search-bar" class="form-control mt-2 me-2" type="search" placeholder="Search" aria-label="Search" autocomplete="" v-model="query" @keyup.enter="showSearchPage(query)">
        </div>
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
        </ul>
        <!-- <div v-if="!isLogin" class="user-menu">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link :to="{ name: 'LogInView' }" class="menu-items"><i class="fa-solid fa-user-plus fa-lg"></i></router-link>
            </li>
          </ul>
        </div>
        <div v-else class="nav-right scroll-down" style="height:60px; margin-top: -.5em;">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link :to="{ name: 'ProfileView', params: {username: currUser?.username} }" class="openbtn menu-items">
                <img 
                class="openbtn "
                id="navbar-profile-img"
                :src="currUser.profile_image ? 'http://127.0.0.1:8000' + currUser.profile_image: require(`@/assets/default.png`)" 
                style="margin-right: -.5em; border-radius: 50%; width: 3.5em; height: 3em;"
                alt="">
                {{ currUser?.nickname }}
              </router-link>
            </li>
            <li class="nav-item">
              <button @click="openNav" class="openbtn menu-items" style="margin-left: -.5em;">☰</button> 
            </li>
            <li class="nav-item">
              <span @click="logOut" class="openbtn logout-btn menu-items">
                <i class="fa-solid fa-right-to-bracket fa-lg" style="backgroundColor: transparent"></i>
              </span>
            </li>
          </ul> -->
        <!-- </div> -->
      </div>
    </nav>

    <!-- <nav :class="navBarStatus ? 'scroll-down' : ''">
      <div class="logo" @click="reload">
        <router-link :to="{ name: 'MainView' }">
          <span>
            neon lights
          </span>
        </router-link>
      </div>
      <div class="search">
        <input id="search-bar" class="form-control mt-2 me-2" type="search" placeholder="Search" aria-label="Search" autocomplete="" v-model="query" @keyup.enter="showSearchPage(query)">
      </div>
      <div v-if="!isLogin" class="user-menu">
        <router-link :to="{ name: 'LogInView' }" class="menu-items"><i class="fa-solid fa-user-plus fa-lg"></i></router-link>
      </div>
      <div v-else class="nav-right scroll-down" style="height:60px; margin-top: -.5em;">
        <router-link :to="{ name: 'ProfileView', params: {username: currUser?.username} }" class="openbtn menu-items">
          <img 
          class="openbtn "
          id="navbar-profile-img"
          :src="currUser.profile_image ? 'http://127.0.0.1:8000' + currUser.profile_image: require(`@/assets/default.png`)" 
          style="margin-right: -.5em; border-radius: 50%; width: 3.5em; height: 3em;"
          alt="">
          {{ currUser?.nickname }}
        </router-link>
        <button @click="openNav" class="openbtn menu-items" style="margin-left: -.5em;">☰</button> 
        <span @click="logOut" class="openbtn logout-btn menu-items">
          <i class="fa-solid fa-right-to-bracket fa-lg" style="backgroundColor: transparent"></i>
        </span>
      </div>
    </nav> -->
    <router-view />
  </div>
</template>


<script>
import { mapState, mapGetters, mapActions } from 'vuex'

export default ({
  name: 'App',
  data() {
    return {
      query: null,
      sidebarStatus: false,
      drawer: null,
      links: [
        ['mdi-send', '내가 좋아요한 영화'],
        ['mdi-inbox-arrow-down', '내가 작성한 리뷰'],
        ['mdi-delete', '팔로잉'],
        ['mdi-alert-octagon', 'Spam'],
      ],
      windowTop: window.top.scrollY,
      navBarStatus: false,
    }
  },  
  computed: {
    ...mapState([
      'currUser',
      'searchCompleted',
    ]),
    ...mapGetters([
      'isLogin',
    ]),
  },
  methods: {
    ...mapActions([
      'showSearchPage',
      'fetchRecommendMovies',
      'fetchNowPlayingMovies',
      'fetchActionMovies',
      'fetchRomanceMovies',
      'getUserLikes',
    ]),
    logOut() {
      if (confirm('로그아웃 하실건가요?') == true){ 
        //true는 확인버튼을 눌렀을 때 코드 작성
        this.$store.dispatch('logOut')
      }
    },
    reload() {
      this.$router.go() // 새로고침
    },
    openNav() {
      this.sidebarStatus = true
    },
    closeNav() {
      this.sidebarStatus = false
    },
    onScroll() {
      this.windowTop = window.top.scrollY
      if (this.windowTop > 50) {
        this.navBarStatus = true
      } else {
        this.navBarStatus = false
      }
    }
  },
  watch: {
    searchCompleted() {
      this.query = null
    }
  },
  mounted() {
    window.addEventListener("scroll", this.onScroll)
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.onScroll)
  }
})

</script>


<style lang="scss">
* { padding:0; margin:0; box-sizing: border-box; text-decoration: none !important; transition: all 0.3s ease 0s !important;}

a:hover {
  color: $primary !important;
}

body {
	padding: 0;
	margin: 0;
  position: relative;
  background-color: $body-bg;
  color: #fff;
}

ul,li,ol{list-style:none; padding: 0 !important;}

.card {
  background-color: $body-bg !important;
}

#app {
  font-family: 'Poppins', 'Noto Sans KR', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #fff;
}

// #navbar-profile-img {
//   position: relative;
// }

a {
  text-decoration:none;
  color:#fff;
  cursor: pointer;
}

nav {
  overflow: hidden;
  padding: 2.7em;
  color: #fff;
  /* background-color: #222222; */
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  /* align-items: center; */
  flex-wrap: wrap;
  position: sticky;
  top: 0;
  z-index: 1;
}

.scroll-down {
 background-color: rgb(0, 0, 0, 0.5)
}

nav a {
  font-weight: bold;
  color: white;
  text-decoration: none;
}

nav a.router-link-exact-active {
  color: $primary;
}

input {
  height: 3em;
  border-radius: 10px;
}

.search {
  margin-top: -.4em;
  width: 30%;

}

.user-menu {

  .menu-items {
    color: white;
    margin-right: 1em;
  }
}

#search-bar {
  color: rgb(240, 229, 229);
}

/* 로고 네온사인 */
:root {
  /* Set neon color */
  --neon-text-color: #bc13fe;
  --neon-border-color: rgb(0, 153, 255);
}

.logo span {
  font-size: 1.2rem;
  font-weight: 200;
  font-style: italic;
  color: #fff;
  margin-left: -.8em;
  padding: 1.2em 2em;
  border: 0.3rem solid #fff;
  border-radius: 2rem;
  text-transform: uppercase;
  animation: flicker 3s infinite alternate;
  ::-moz-selection {
    background-color: var(--neon-border-color);
    color: var(--neon-text-color);
  }
  ::selection {
    background-color: var(--neon-border-color);
    color: var(--neon-text-color);
  }
  :focus {
    outline: none;
  }
}

/* Animate neon flicker */
@keyframes flicker {
  0%, 19%, 21%, 23%, 25%, 54%, 56%, 100% {
      
        text-shadow:
            -0.2rem -0.2rem 1rem #fff,
            0.2rem 0.2rem 1rem #fff,
            0 0 2rem var(--neon-text-color),
            0 0 4rem var(--neon-text-color),
            0 0 6rem var(--neon-text-color),
            0 0 8rem var(--neon-text-color),
            0 0 10rem var(--neon-text-color);
        
        box-shadow:
            0 0 .5rem #fff,
            inset 0 0 .5rem #fff,
            0 0 1.5rem var(--neon-border-color),
            inset 0 0 1.2rem var(--neon-border-color),
            0 0 1.5rem var(--neon-border-color),
            inset 0 0 .5rem var(--neon-border-color);        
    }
    
    20%, 24%, 55% {        
        text-shadow: none;
        box-shadow: none;
    }    
}

.logout-btn {
  cursor: pointer;
}

.sidebar {
  height: 100%; /* 100% Full-height */
  width: 0; /* 0 width - change this with JavaScript */
  position: fixed; /* Stay in place */
  z-index: 3; /* Stay on top */
  top: 0;
  right: 0;
  background-color: #111; /* Black*/
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 60px; /* Place content 60px from the top */
  transition: 0.5s; /* 0.5 second transition effect to slide in the sidebar */
}

.sidebar-opened {
  width: 250px
}

.sidebar-closed {
  width: 0
}

.main-opened {
  margin-right: 250px;
}

.main-closed {
  margin: 0;
}

.sidebar .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  margin-left: 50px;
}

.openbtn {
  font-size: 20px;
  cursor: pointer;
  background-color: $body-bg;
  color: white;
  padding: 10px 15px;
  border: none;
}
</style>
