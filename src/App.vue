<template>
  <div id="app">
    <h1>ToDoリスト</h1>

    <template v-for="(option, index) in options">
      <label :key="index">
        <input
          type="radio"
          name="status"
          v-model="checkStatus"
          :value="option.value"
          @change="displaySwitching"
        />{{ option.label }}
      </label>
    </template>

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(toDo, index) in toDos" :key="index">
          <td>{{ toDo.id }}</td>
          <td>{{ toDo.task }}</td>
          <td>
            <button @click="changeStatus(toDo.status, toDo.id)">
              {{ toDo.status }}
            </button>
          </td>
          <td><button @click="deleteTask(toDo.id)">削除</button></td>
        </tr>
      </tbody>
    </table>

    <input type="text" v-model="newTask" />
    <button @click="addNewTask">追加</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      checkStatus: "all",
      newTask: "",
      /* タスクリスト表示用の配列 */
      toDos: [],
      /* タスクリストのマスターデータ用配列 */
      masterToDos: [],
      options: [
        {
          label: "全て",
          value: "all",
        },
        {
          label: "作業中",
          value: "inWork",
        },
        {
          label: "完了",
          value: "completed",
        },
      ],
    };
  },
  methods: {
    displaySwitching() {
      if (this.checkStatus === "all") {
        this.toDos = this.masterToDos;
      } else if (this.checkStatus === "inWork") {
        this.toDos = this.masterToDos.filter((value) => {
          return value.status === "作業中";
        });
      } else {
        this.toDos = this.masterToDos.filter((value) => {
          return value.status === "完了";
        });
      }
    },
    addNewTask() {
      const toDo = {
        id: this.toDos.length,
        task: this.newTask,
        status: "作業中",
      };
      this.toDos.push(toDo);
      this.masterToDos = this.toDos;
      this.newTask = "";
      this.displaySwitching();
    },
    changeStatus(status, id) {
      if (status === "作業中") {
        this.masterToDos[id].status = "完了";
        this.assignToDosToMasterToDos();
      } else {
        this.masterToDos[id].status = "作業中";
        this.assignToDosToMasterToDos();
      }
    },
    deleteTask(id) {
      this.masterToDos.splice(id, 1);
      /* 以下の記述は、タスク削除後にIDを0から振り直すためのコードです */
      for (let i = 0; i < this.masterToDos.length; i++) {
        this.masterToDos[i].id = i;
      }
      this.assignToDosToMasterToDos();
    },
    assignToDosToMasterToDos() {
      this.toDos = this.masterToDos;
      this.displaySwitching();
    },
  },
};
</script>