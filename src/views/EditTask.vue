<template>
  <form @submit.prevent="handleSubmit">
    <label for="title">Title:</label>
    <input type="text" required id="title" v-model="title" />

    <label for="details">Details:</label>
    <textarea id="details" required v-model="details"></textarea>

    <button>Update task</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/tasks/",
    };
  },
  props: ["id"],
  mounted() {
    fetch(this.uri + this.$props.id)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      });
  },
  methods: {
    handleSubmit() {
      fetch(this.uri + this.$props.id, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ title: this.title, details: this.details }),
      })
        .then(() => this.$router.push("/"))
        .catch((err) => console.error(err));
    },
  },
};
</script>

<style></style>
