<template>
  <div class="task" :class="{ completed: task.completed }">
    <div class="actions">
      <h3 @click="isExtended = !isExtended">{{ task.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditTask', params: { id: task.id } }">
          <span class="material-symbols-outlined"> edit </span>
        </router-link>
        <span class="material-symbols-outlined" @click="deleteTask">
          delete
        </span>
        <span class="material-symbols-outlined click" @click="toggleComplete">
          done
        </span>
      </div>
    </div>
    <div class="details" v-if="isExtended">
      <p>{{ task.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["task"],
  data() {
    return {
      isExtended: false,
      uri: " http://localhost:3000/tasks/" + this.task.id,
    };
  },
  methods: {
    deleteTask() {
      fetch(this.uri, { method: "DELETE" })
        .then(() => {
          this.$emit("delete", this.task.id);
        })
        .catch((err) => console.error(err));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ completed: !this.task.completed }),
      })
        .then(() => {
          this.$emit("completed", this.task.id);
        })
        .catch((err) => console.error(err));
    },
  },
};
</script>

<style>
.task {
  margin: 20px auto;
  background: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
.task.completed {
  border-left: 4px solid #00ce89;
}
.task.completed .click {
  color: #00ce89;
}
h3 {
  cursor: pointer;
}
.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-symbols-outlined {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-symbols-outlined:hover {
  color: #777;
}
</style>
