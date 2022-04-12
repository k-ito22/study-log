<template>
  <div class="main">
    <h1>Study-Log</h1>
    <div class="main-page">
      <div class="left">
        <AddLog
          @addLog="addLogCard"
        />
        <LogCard 
          v-for="(log, index) in sortedLogList"
          :key="log.id"
          :logData="log"
          :index="index"
          @removeCard="removeLog"
        />
      </div>
      <div class="right">
        <p class="total">合計時間: {{ total }} 時間</p>
        <div class="issue">
          <p class="issue-title">課題リスト</p>
          <div class="issue-list">
            <div class="remaining">
              <p>未解決</p>
              <draggable tag="ul" group="issues" v-model="issueList">
                <li v-for="issue in issueList"
                :key="issue.id">
                {{ issue }}
                </li>
              </draggable>
            </div>
            <div class="solved">
              <p>解決済</p>
              <draggable tag="ul" group="issues" v-model="solvedIssueList">
                <li v-for="issue in solvedIssueList"
                :key="issue.id">
                {{ issue }}
                </li>
              </draggable>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AddLog from '@/components/AddLog'
import LogCard from '@/components/LogCard'
import draggable from 'vuedraggable'

export default {
  data: function() {
    return {
      logList: [],
      issueList: [],
      solvedIssueList: [],
    }
  },
  created: function() {
    const logData = localStorage.getItem('logList');
    if (logData != null) {
      this.logList = JSON.parse(logData);
      const issueData = localStorage.getItem('issueList');
      if (issueData != null) {
        this.issueList = JSON.parse(issueData);
      }
      const solvedIssueData = localStorage.getItem('solvedIssueList');
      if (solvedIssueData != null) {
        this.solvedIssueList = JSON.parse(solvedIssueData);
      }
    }

  },
  components: {
    AddLog,
    LogCard,
    draggable,
  },
  methods: {
    addLogCard: function(date, time, issues) {
      this.logList.push({
        date: date,
        time: time,
        issues: issues,
      });
      issues.forEach((issue) => {
        this.issueList.push(issue);
      })
    },
    removeLog: function(index) {
      const deleteIssues = this.logList[index].issues;
      deleteIssues.forEach(deleteIssue => {
        const index = this.issueList.indexOf(deleteIssue);
        if(index === -1) {
          const solvedIndex = this.solvedIssueList.indexOf(deleteIssue);
          this.solvedIssueList.splice(solvedIndex, 1);
        } else {
          this.issueList.splice(index, 1);
        }
      });
      this.logList.splice(index, 1);
      
    }
  },
  computed: {
    total: function() {
      let sum = 0;
      this.logList.forEach(log => {
        sum += log.time;
      });
      return sum;
    },
    sortedLogList: function() {
      return this.logList.slice().sort((a, b) => {
        return a.date > b.date ? -1 : 1;
      });
    }
  },
  watch: {
    logList: {
      handler: function(logList) {
        localStorage.setItem('logList', JSON.stringify(logList));
      }
    },
    issueList: {
      handler: function(issueList) {
        localStorage.setItem('issueList', JSON.stringify(issueList));
      }
    },
    solvedIssueList: {
      handler: function(solvedIssueList) {
        localStorage.setItem('solvedIssueList', JSON.stringify(solvedIssueList));
      }
    },
  }
}
</script>

<style scoped>
h1 {
  margin-bottom: 40px;
}
.main-page {
  display: flex;
  margin-bottom: 20px;
  height: 100%;
}
.main {
  height: 100%;
}
.left {
  width: 350px;
  margin-left: 10px;
}
.right {
  flex-grow: 1;
  height: 100%;
  margin-right: 10px;
}
.total {
  margin-top: 0;
  font-size: 24px;
}
.issue {
  height: 100%;
}
.issue-title {
  margin-bottom: 0;
  font-weight: bold;
}
.issue-list {
  display: flex;
  height: 100%;
}
.remaining {
  width: 50%;
  margin: 10px;
  border-radius: 12px;
  min-height: 90%;
  overflow: scroll;
  background-color: white;
}
.solved {
  width: 50%;
  margin: 10px;
  border-radius: 12px;
  min-height: 90%;
  overflow: scroll;
  background-color: white;
}
ul {
  padding-left: 0;
  height: 100%;
}
li {
  text-align: left;
  list-style: none;
  width: 80%;
  margin: 0 auto;
  padding: 5px 10px;
  border-radius: 4px;
  margin-bottom: 5px;
}
.remaining li {
  background-color:rgb(241, 189, 189)
}

.solved li {
  background-color: rgb(163, 220, 186);
}
</style>