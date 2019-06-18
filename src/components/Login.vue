<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 mt-5 mx-auto">
                <form v-on:submit.prevent="login">
                    <h1 class="h3 mb-3 font-weight-normal">Please sign in</h1>
                    <div class="form-group">
                        <label for="username">Username</label>
                        <input type="username" v-model="username" class="form-control" name="username" placeholder="Enter username">
                    </div>
                    <div class="form-group">
                        <label for="password">Password</label>
                        <input type="password" v-model="password" class="form-control" name="password" placeholder="Enter Password">
                    </div>
                    <button class="btn btn-lg btn-primary btn-block">Login</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>/*eslint-disable */
import axios from 'axios'
import router from '../router'
import EventBus from './EventBus'
import jwt_decode from 'jwt-decode'
export default {
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login () {
      axios.post('/api/auth', {
        username: this.username,
        password: this.password
      }).then((res) => {
        var decoded = jwt_decode(res.data.data.token)
        console.log(decoded.payload['0'])
        localStorage.setItem('userInfo', JSON.stringify(decoded.payload['0']))
        localStorage.setItem('auth', res.data.data.token)
        this.username = ''
        this.password = ''
        router.push({ name: 'Profile' })
      }).catch((res, err) => {
        console.log(res)
        console.log(this.username)
        console.log(this.password)
        console.log(err)
      })
      this.emitMethod()
    },
    emitMethod () {
      EventBus.$emit('auth',true)
    }
  }
}

</script>
