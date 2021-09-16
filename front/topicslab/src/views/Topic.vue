<template>
  <div>
    <Card>
      <template #title>
        {{topic.title}}
        <Skeleton v-if="seen" height="40px" />
      </template>
      <template #content>
        <div class="body-text">
          {{topic.body}}
          <Skeleton v-if="seen" style="margin-bottom:5px" />
          <Skeleton v-if="seen" />
        </div>
      </template>
      <template #footer>
        <span>
          <router-link :to="`/user/${user.id}`">{{user.name}}</router-link>
          <Skeleton v-if="seen" style="width: 60px;float: right"/>
        </span>
        <div class="button">
          <Button icon="pi pi-heart" class="p-button-rounded p-button-help p-mr-2 p-mb-2" />
        </div>
      </template>
    </Card>
    <Comments :comments="this.comments" />
    <CommentForm :topicId="this.topic.id" @sentComment="receiveComment" />
  </div>

</template>

<script>
import axios from '@/supports/axios'
import Comments from '@/components/Comments'
import CommentForm from '@/components/CommentForm'

export default {
  name: 'Topic',
  components: {
    Comments,
    CommentForm
  },
  data () {
    return {
      topic: {},
      user: {},
      comments: [],
      id: null,
      seen: true
    }
  },
  mounted () {
    this.id = this.$route.params.id
    if (!this.id) {
      alert('不正なIDです。')
    }
    this.getTopic()
  },
  methods: {
    getTopic () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.get(`/api/topic/${this.id}`)
            .then((res) => {
              if (res.status === 200 && res.data.length === 1) {
                this.topic = res.data[0]
                this.user = this.topic.user
                this.comments.splice(0)
                this.comments.push(...this.topic.comments)
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
    },
    receiveComment (comment) {
      this.comments.push(comment)
    }
  }
}
</script>

<style scoped>
.body-text {
  white-space:pre-wrap;
}
.p-card-footer span {
  text-align: right;
  display: block;
}
.button {
  text-align: right;
  margin-top: 10px;
}
</style>
