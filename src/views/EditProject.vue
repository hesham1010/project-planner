<template>
  <form @submit.prevent="submitHandler">
    <label for="">Title:</label>
    <input type="text" v-model="title" required />
    <label for="">Details:</label>
    <textarea v-model="details" required></textarea>
    <button type="submit">Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/projects/" + this.id,
    };
  },
  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      });
  },
  methods: {
    submitHandler() {
      let project = {
        title: this.title,
        details: this.details,
      };
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project),
      })
        .then(() => this.$router.push("/"))
        .catch((e) => console.log(e.message));
    },
  },
};
</script>

<style>
</style>