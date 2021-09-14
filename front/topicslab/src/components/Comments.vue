<template>
  <div>
    <Fieldset v-for="comment in comments" :key="comment.id">
      <template #legend>
        <span>{{comment.user.name}}</span>
      </template>
      <div class="comment-text">
        {{comment.body}}
      </div>
      <div class="button">
          <Button icon="pi pi-heart" class="p-button-rounded p-button-help p-mr-2 p-mb-2" />
      </div>
    </Fieldset>
  </div>
</template>

<script>
export default {
  name: 'Comments',
  props: {
    comments: Array
  }
}
</script>

<style lang="scss" scoped>
.p-fieldset {
  margin-top: 20px;
}

.comment-text {
  white-space:pre-wrap;
}
.button {
  text-align: right;
  margin-top: 10px;
}
</style>
