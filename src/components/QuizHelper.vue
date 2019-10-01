<template>
  <v-container grid-list-lg>
    <v-layout column>
      <v-flex>
        <v-card>
          <v-card-text>
            <v-layout text-center wrap>
              <v-flex xs12>
                <v-text-field v-model="question" abel="Question" />
              </v-flex>
              <v-flex xs12>
                <v-layout>
                  <v-flex xs8>Answers</v-flex>
                  <v-flex>Weights</v-flex>
                </v-layout>
              </v-flex>
              <v-flex v-for="(answer, index) in answers" :key="index" xs12>
                <v-layout>
                  <v-flex xs8>
                    <v-text-field v-model="answers[index].text" :label="'Answer #' + (index + 1)" />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.gryffindor"
                      :class="answers[index].weights.gryffindor > 0 ? 'gryffindor' : ''"
                      label="Gryffindor"
                      color="red darken-3"
                    />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.ravenclaw"
                      label="Ravenclaw"
                      color="blue darken-2"
                    />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.slytherin"
                      label="Slytherin"
                      color="green darken-3"
                    />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.hufflepuff"
                      label="Hufflepuff"
                      color="lime darken-2"
                    />
                  </v-flex>
                </v-layout>
              </v-flex>
            </v-layout>
          </v-card-text>
        </v-card>
      </v-flex>
      <v-flex>
        <v-card>
          <v-card-text>
              <code>
                {{generatedCode}}
              </code>
          </v-card-text>
          <v-card-actions>
            <v-btn>Copy to Clipboard</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
const houses = {
  gryffindor: "Gryffindor",
  ravenclaw: "Ravenclaw",
  slytherin: "Slytherin",
  hufflepuff: "Hufflepuff"
};

export default {
  data() {
    return {
      question: "Are you Harry Potter?",
      answers: [
        { text: "Yes", weights: { gryffindor: 0.7, slytherin: 0.3 } },
        { text: "...yes (sneakily)", weights: { slytherin: 1 } },
        {
          text: "No",
          weights: {
            ravenclaw: 0.25,
            gryffindor: 0.25,
            slytherin: 0.25,
            hufflepuff: 0.25
          }
        },
        { text: "I wish", weights: { hufflepuff: 1 } }
      ]
    };
  },
  computed: {
    generatedCode() {
      const lines = [];
      lines.push(`new Question(\`${this.sanitize(this.question)}\`, [`);
      this.answers.forEach(answer => {
        let weightStr = Object.keys(answer.weights)
          .map(key => {
            return `new Weight(House.${houses[key]}, ${answer.weights[key]})`;
          })
          .join(", ");
        lines.push(
          `  new Answer(\`${this.sanitize(answer.text)}\`, [${weightStr}]),`
        );
      });
      lines.push("])");
      return lines.join("\n");
    }
  },
  methods: {
    sanitize(str) {
      return str.replace(/`/g, "");
    }
  }
};
</script>

<style>
/* .gryffindor input {
  background-color: #C62828 ;
  text-align: center;
  color: white !important;
} */
</style>