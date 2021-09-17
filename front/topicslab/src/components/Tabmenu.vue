<template>
  <div>
    <TabView>
      <TabPanel header="トピック">
    <Card v-for="user in users" :key="user.id">
        <template #content>
          <h2>
            <!-- <router-link :to="`/topic/${this.id}`">
              {{user.name}}
            </router-link> -->
          </h2>
        </template>
    </Card>
    <Card v-if="seen">
    <template #content>
      <Skeleton width="200px" style="margin-bottom:10px"/>
      <Skeleton height="50px"/>
    </template>
    </Card>
      </TabPanel>
      <TabPanel header="コメント">
        コメント
      </TabPanel>
    </TabView>
  </div>
</template>

<script>
import axios from '@/supports/axios'

export default {
  name: 'Tabmenu',
  data () {
    return {
      users: [],
      seen: true
    }
  },
  mounted () {
    this.getTabmenu()
    // this.id = this.$route.params.id
    // if (!this.id) {
    //   alert('不正なIDです。')
    // }
  },
  methods: {
    getTabmenu () {
      // axios.get('/sanctum/csrf-cookie')
      //   .then(() => {
      //     axios.get(`/api/user/${this.id}`)
      //       .then((res) => {
      //         if (res.status === 200) {
      //           this.users.splice(0)
      //           this.users.push(res.data)
      //           this.seen = false
      //         } else {
      //           console.log('取得失敗')
      //         }
      //       })
      //   })
      //   .catch((err) => {
      //     alert(err)
      //   })
    }
  }
}
</script>
