<template>
  <div>
    <Card>
      <template #content>
        {{user.name}}
        <Skeleton v-if="seen" />
        <Tabmenu />
      </template>
    </Card>
  </div>
</template>

<script>
import axios from '@/supports/axios'
import Tabmenu from '@/components/Tabmenu'/* eslint-disable */

export default {
  name: 'user',
  components: {
  Tabmenu/* eslint-disable */
  },
  data () {
    return {
      id: null,
      user: {},
      seen: true
    }
  },
  mounted () {
    if (localStorage.getItem('authenticated') !== 'true') {
      this.$router.push('../login')
      return
    }

    this.id = this.$route.params.id
    if (!this.id) {
      alert('不正なIDです。')
    }
    this.getUser()
  },
  methods: {
    getUser () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.get(`/api/user/${this.id}`)
            .then((res) => {
              console.log(res)
              if (res.status === 200) {
                this.user = res.data
                this.seen = false
              } else {
                console.log('取得失敗')
              }
            })
            .catch((err) => {
              console.log(err)
              alert(err)
            })
        })
        .catch((err) => {
          alert(err)
        })
    }
  }
}
</script>
