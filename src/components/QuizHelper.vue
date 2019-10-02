<template>
  <v-container grid-list-lg>
    <v-layout column>
      <v-flex>
        <v-card>
          <v-card-title>
            How to Contribute
          </v-card-title>
          <v-card-text>
            <ol>
              <li>Create a question using the below form</li>
              <ol type="a">
                <li>
                  A question has four answers, each with some combination of weights indicating which House the question is targeting
                </li>
                <li>
                  Weights are relative to each other within the question, and don't have to add up to anything in particular
                </li>
              </ol>
              <li>
                Copy the generated code to your clipboard
              </li>
              <li>
                Paste what you've copied at the end of the <tt>questions</tt> array <a href="https://github.com/nrabins/hp-quiz/edit/master/src/data/questions.ts" target="_blank">here</a>
              </li>
              <li>
                Repeat the above steps to make more questions, if desired
              </li>
              <li>
                Commit your changes using the interface at the bottom of the github edit page
              </li>
            </ol>
          </v-card-text>
        </v-card>
      </v-flex>
      <v-flex>
        <v-card>
          <v-card-text>
            <v-layout text-center wrap>
              <v-flex xs12>
                <v-text-field v-model="question" ref="question" label="Question" />
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
                      type="number"
                      min="0"
                      step=".1"
                      max="1"
                    />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.ravenclaw"
                      label="Ravenclaw"
                      color="blue darken-2"
                      type="number"
                      min="0"
                      step=".1"
                      max="1"
                    />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.slytherin"
                      label="Slytherin"
                      color="green darken-3"
                      type="number"
                      min="0"
                      step=".1"
                      max="1"
                    />
                  </v-flex>
                  <v-flex xs1>
                    <v-text-field
                      v-model="answers[index].weights.hufflepuff"
                      label="Hufflepuff"
                      color="lime darken-2"
                      type="number"
                      min="0"
                      step=".1"
                      max="1"
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
            <pre class="code" ref="generatedCode">{{generatedCode}}</pre>
          </v-card-text>
          <v-card-actions>
            <v-btn @click="resetForm">Reset Form</v-btn>
            <v-spacer />
            <v-btn @click="copyToClipboard">Copy to Clipboard</v-btn>
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
      question: "",
      answers: []
    };
  },
  mounted() {
    this.resetForm();
  },
  computed: {
    generatedCode() {
      const lines = [];
      lines.push(`  new Question(\`${this.sanitize(this.question)}\`, [`);
      this.answers.forEach(answer => {
        const weightKeysWithValues = Object.keys(answer.weights).filter(
          key => answer.weights[key] > 0
        );

        let weightStr = "";

        if (weightKeysWithValues.length === 1) {
          weightStr = `new Weight(House.${houses[weightKeysWithValues[0]]})`;
        } else {
          weightStr = weightKeysWithValues
            .map(key => {
              return `new Weight(House.${houses[key]}, ${answer.weights[key]})`;
            })
            .join(", ");
        }
        lines.push(
          `    new Answer(\`${this.sanitize(answer.text)}\`, [${weightStr}]),`
        );
      });
      lines.push("  ]),");
      return lines.join("\n");
    }
  },
  methods: {
    sanitize(str) {
      return str.replace(/`/g, "");
    },
    copyToClipboard() {
      const range = document.createRange();
      range.selectNodeContents(this.$refs.generatedCode);
      const sel = window.getSelection();
      sel.removeAllRanges();
      sel.addRange(range);
      document.execCommand("copy");
      sel.removeAllRanges();
    },
    resetForm() {
      this.question = "";
      this.answers = [
        { text: "", weights: {} },
        { text: "", weights: {} },
        { text: "", weights: {} },
        { text: "", weights: {} }
      ];
      this.$refs.question.focus();
    }
  }
};
</script>

<style>
.code {
  overflow: auto;
}
/* .gryffindor input {
  background-color: #C62828 ;
  text-align: center;
  color: white !important;
} */
</style>