<template>
    <v-app-bar color="green-darken-1">
      <v-icon class="ml-6">mdi-calendar-check</v-icon>
      <v-app-bar-title class="ml-2">To-do-list</v-app-bar-title>

      <v-card-text style="margin-right: 28%;">
        <v-text-field
          :loading="loading"
          append-inner-icon="mdi-magnify"
          density="compact"
          label="Pesquisar tarefa"
          variant="solo"
          hide-details
          single-line
          @click:append-inner="onClick"

        ></v-text-field>
      </v-card-text>

      <template v-slot:append>
        <v-btn icon="mdi-dots-vertical"></v-btn>
      </template>
    </v-app-bar>

</template>

<script>
export default {
  data() {
    return {
      loading: false
    };
  },
  methods: {
    onClick() {
      this.loading = true;

      setTimeout(() => {
        this.loading = false;
      }, 2000);
    }
  }
};
</script>
