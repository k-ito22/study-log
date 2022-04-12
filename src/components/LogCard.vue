<template>
  <div class="log-card">
    <div class="header">
      <p class="title">{{ logData.date }}</p>
      <div class="delete-card" @click="onRemoveCard(index)">x</div>
    </div>
    <p class="time">勉強時間： {{ logData.time }} 時間</p>
    <div class="issue">
      <div>
        <p>課題点等： </p>
      </div>
      <div>
        <p v-for="issue in logData.issues"
          :key="issue.id">
          {{ issue }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    logData: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    }
  },
  methods: {
    onRemoveCard: function(index) {
      this.$emit('removeCard', index)
    }
  }
}
</script>

<style scoped>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.log-card {
  text-align: left;
  margin-top: 15px;
  display: inline-block;
  width: 300px;
  background-color: #e0e0e0;
  border-radius: 16px;
  padding: 0 15px;
}
.title {
  font-weight: bold;
  margin-bottom: 0;
}
.delete-card {
  margin-right: 5px;
  cursor: pointer;
}.issue {
  display: flex;
}
.issue p {
  margin-top: 0;
  margin-bottom: 10px;
}
</style>