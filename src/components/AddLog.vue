<template>
  <div>
    <form @submit.prevent="onAddLog(date, time, issues)" class="add-log">
      <input type="date"
        class="date-input"
        v-model="date">
      <div class="main">
        <div>
          <input type="number"
            class="number-input"
            v-model.number="time"
          >
          時間
        </div>
        <div>
          <div class="icon" @click="addIssue">
            Add Issue
            <i class="fas fa-plus-circle"></i>
          </div>
        </div>
      </div>
      <input 
        type="text"
        class="text-input"
        placeholder="remaining issue"
        v-for="(issue, index) in issues"
        :key="issue.id"
        v-model="issues[index]"
      />
      <button class="add-button"
        type="submit"
        v-bind:class="{addable: this.date && this.time >= 0}"
      >
        登録
      </button>
    </form>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      date: '',
      time: 0,
      issues: [],
    }
  },
  components: {
  },
  methods: {
    addIssue: function() {
      this.issues.push('');
    },
    onAddLog: function(date, time, issues) {
      issues = issues.filter(issue => issue.length > 0);
      if (this.date && this.time >= 0) {
        this.$emit('addLog', date, time, issues);
        this.date = '';
        this.time = 0;
        this.issues = [];
      }
    },
  },
}
</script>

<style scoped>
.add-log {
  margin: 0 10px auto;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 320px;
}
.date-input {
  padding:8px;
  width: calc(100% - 120px);
  background-color: #ccc;
  border-radius: 8px;
  cursor: pointer;
  border: none;
  font-family: "Note Sans Japanese", "Noto Sans", 'system-ui', sans-serif;
  font-weight: 700;
  font-size: 16px;
  color: #242424;
  cursor: pointer;
  overflow: overlay;
}
.number-input {
  padding: 10px;
  width: 40px;
  margin: 15px 0;
  background-color: #ccc;
  border-radius: 8px;
  cursor: pointer;
  border: none;
  font-family: "Note Sans Japanese", "Noto Sans", 'system-ui', sans-serif;
  font-weight: 700;
  font-size: 16px;
  color: #242424;
  cursor: pointer;
  overflow: overlay;
}
.text-input {
  padding: 10px;
  margin-bottom: 15px;
  width: calc(100% - 80px);
  background-color: #ccc;
  border-radius: 8px;
  cursor: pointer;
  border: none;
  font-family: "Note Sans Japanese", "Noto Sans", 'system-ui', sans-serif;
  font-weight: 700;
  font-size: 16px;
  color: #242424;
  cursor: pointer;
  overflow: overlay;
}
.main {
  width: 240px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.add-button {
  padding: 10px;
  margin-bottom: 10px;
  background-color: #999;
  border: none;
  border-radius: 8px;
  font-family: "Noto Sans Japanese", "Noto Sans", 'system-ui', sans-serif;
  font-weight: 700;
  font-size: 16px;
  color: #fff;
}
.addable {
  background-color: #288e15;
  pointer-events: auto;
  cursor: pointer;
}
</style>