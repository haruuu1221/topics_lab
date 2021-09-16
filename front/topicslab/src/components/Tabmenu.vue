<template>
  <div>
    <TabView>
      <TabPanel header="トピック">
    <Card v-for="user in users" :key="user.id">
        <template #content>
          <h2>
            <router-link :to="`/topic/${user.id}`">
              {{ user.name }}
            </router-link>
          </h2>
        </template>
    </Card>
    <Skeleton v-if="seen" height="150px" />
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
  },
  methods: {
    getTabmenu () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.get('/api/user')
            .then((res) => {
              if (res.status === 200) {
                this.users.splice(0)
                this.users.push(res.data)
                this.seen = false
              } else {
                console.log('取得失敗')
              }
            })
        })
        .catch((err) => {
          alert(err)
        })
    }
  }
}
</script>
