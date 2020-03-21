<template>
  <form-create />
</template>

<script>
import FormCreate from "@/components/FormCreate";
export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  },
  components: {
    FormCreate
  }
};
</script>
