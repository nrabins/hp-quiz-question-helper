<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>HP Quiz question helper</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://https://github.com/nrabins/hp-quiz"
        target="_blank"
      >
        <span class="mr-2">HP Quiz Github</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <QuizHelper/>
    </v-content>
  </v-app>
</template>

<script>
import QuizHelper from '@/components/QuizHelper';

export default {
  name: 'App',
  components: {
    QuizHelper,
  },
};
</script>
