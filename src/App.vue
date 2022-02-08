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
        <tr v-for="(toDo, index) in filterToDos" :key="index">
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
      toDos: [],
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
  computed: {
    filterToDos() {
      if (this.checkStatus === "all") return this.toDos;
      if (this.checkStatus === "inWork")
        return this.toDos.filter((value) => value.status === "作業中");
      return this.toDos.filter((value) => value.status === "完了");
    },
  },
  methods: {
    addNewTask() {
      const toDo = {
        id: this.toDos.length,
        task: this.newTask,
        status: "作業中",
      };
      this.toDos.push(toDo);
      this.newTask = "";
    },
    changeStatus(status, id) {
      if (status === "作業中") {
        this.toDos[id].status = "完了";
      } else {
        this.toDos[id].status = "作業中";
      }
    },
    deleteTask(id) {
      this.toDos.splice(id, 1);
      /* 以下の記述は、タスク削除後にIDを0から振り直すためのコード */
      this.toDos.forEach((currentValue, index) => {
        currentValue.id = index;
      });
    },
  },
};
</script>