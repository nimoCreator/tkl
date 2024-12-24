<template>
  <table>
    <thead>
      <tr>
        <th>var</th>
        <th v-for="i in inputWidth" :key="i">
          {{ i - 1 }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(row, rowIndex) in input" :key="rowIndex">
        <td>
          {{ row.var }}
        </td>
        <td
          v-for="(cell, columnIndex) in row.przebieg"
          :key="`${rowIndex}-${columnIndex}`"
        >
          <select v-model="row.przebieg[columnIndex]">
            <option value=""></option>
            <option value="+">+</option>
            <option value="-">-</option>
          </select>
        </td>
      </tr>

      <tr v-for="(row, rowIndex) in inputAnalysis" :key="rowIndex">
        <td>
          {{ row.var }}
        </td>
        <td
          v-for="(cell, columnIndex) in row.przebieg"
          :key="`${rowIndex}-${columnIndex}`"
        >
          {{ cell }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  data() {
    return {
      input: [
        {
          var: "a",
          przebieg: [
            "+",
            " ",
            "-",
            "+",
            " ",
            "-",
            " ",
            " ",
            " ",
            " ",
            " ",
            " ",
            "+",
            " ",
            "-",
            " ",
            " ",
            " ",
            " ",
            " ",
            " ",
            "+",
            " ",
            "-",
          ],
          output: false,
        },
        {
          var: "b",
          przebieg: [
            "-",
            " ",
            " ",
            " ",
            " ",
            " ",
            "+",
            " ",
            "-",
            "+",
            " ",
            "-",
            " ",
            " ",
            " ",
            "+",
            " ",
            "-",
            "+",
            " ",
            "-",
            " ",
            " ",
            " ",
          ],
          output: false,
        },
        {
          var: "w",
          przebieg: [
            "-",
            "+",
            " ",
            " ",
            "-",
            " ",
            " ",
            "+",
            " ",
            " ",
            "-",
            " ",
            " ",
            "+",
            " ",
            " ",
            "-",
            " ",
            " ",
            "+",
            " ",
            " ",
            "-",
            " ",
          ],
          output: true,
        },
      ],
      inputAnalysis: [],
      inputWidth: 24,
    };
  },
  computed: {
    inputAnalysis() {
      let obj = [];
      let nsu = [];
      let sum = 0;
      for (let j = 0; j < this.input.length; j++) 
        if (this.input[j].przebieg[0] == "+") 
          sum += Math.pow(2, j);
      nsu.push(sum);

      for (let i = 1; i < this.inputWidth; i++) {
        for (let j = 0; j < this.input.length; j++) {
          if (this.input[j].przebieg[i] == "+") {
            sum += Math.pow(2, j);
          } else if (this.input[j].przebieg[i] == "-") {
            sum -= Math.pow(2, j);
          }
        }
        nsu.push(sum);
      }
      obj.push({ var: "nsu", przebieg: nsu });

      for (let v = 0; v < this.input.length; v++) {
        if (!this.input[v].output) continue;

        let przebieg = [];
        let state = false;
        if (this.input[v].przebieg[0] == "+") {
          state = true;
        }

        for (let i = 0; i < this.inputWidth; i++) {
          if (this.input[v].przebieg[i + 1] == "+") {
            state = true;
          } else if (this.input[v].przebieg[i + 1] == "-") 
            state = false;

          przebieg.push(state ? "+" : "-");
        }
        obj.push({ var: "i"+this.input[v].var, przebieg });
      }

      return obj;
    },
  },
};
</script>

<style scoped>
* {
  font-family: Lato;
}
table {
  border-collapse: collapse;
  border: 1px solid black;
}
table td {
  border: 1px solid black;
  width: 2rem;
  height: 2rem;
  text-align: center;
}
table select {
  all: unset;
  font-size: 1.25rem;
  font-weight: 900;
}
</style>
