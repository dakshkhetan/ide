<template>
  <span v-if="userStore.isAuthenticated">
   <div class="btn-group" :class="{ open : isOpen}"  @click="open">
      <button type="button" class="btn btn-sm btn-menu"
          aria-haspopup="true" aria-expanded="false" @blur="close" >
        <i class="fa fa-user"></i>
        {{userStore.currentUser.firstname}} 
        <i class="fa fa-caret-down"></i>
      </button>
      <ul class="dropdown-menu">
        <li>
          <router-link class="btn btn-sm btn-menu" tag="button" to="/profile">
            <i class="fa fa-list" aria-hidden="true"></i>
            Saved Codes 
          </router-link>
        </li>
        <li>
          <button type="button" class="btn btn-sm btn-menu" @click="logout">
            <i class="fa fa-sign-out" aria-hidden="true"></i>
            Logout
          </button>
        </li>
      </ul>
    </div>
  </span>
  <button id="panelLang" type="button" class="btn btn-sm btn-danger"
    @click="login"
    v-else >
    Login <i class="fas fa-sign-in-alt"></i>
  </button>
  
</template>

<script>
import { mapState } from 'vuex'
import { setToken } from '@/utils/api'

export default {
  data() {
    return {
      isOpen: false
    }
  },
  computed: mapState({
    userStore: 'user'
  }),
  methods: {
    login () {
      const oneauth = process.env.ONEAUTH
      window.location.href= oneauth.url + `/oauth/authorize?response_type=code&client_id=${oneauth.clientId}&redirect_uri=${process.env.url}callback`
    },
    logout () {
      this.$store.commit('user/logout')
      setToken(null)
      this.$notify({
        text: 'Logged you out. You may still keep the fiddling with code and use the ide in anonymous mode.',
        type: 'success'
      })
    },
    open() {
      this.isOpen = !this.isOpen 
    },
    close () {
      setTimeout(() => { this.isOpen=false }, 250 )
    }
  }
  
}
</script>

<style scoped>
  .open > .dropdown-menu {
    display: list-item !important;
    background-color: #202020;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    font-size: 14px;
    overflow: hidden;
  }

  .open > .dropdown-menu > li:hover {
    cursor: pointer;
  }
</style>
