<template>
  <v-container>
    <v-card elevation="2">
      <v-form class="pa-4 xs6" ref="form" lazy-validation>
        <v-file-input
          chips
          counter
          show-size
          accept=".csv"
          label="File input"
          outlined
          dense
          id="file"
          class="required"
          required
        ></v-file-input>
        <v-btn color="error" class="mr-4" @click="submitFile">
          Submit Form
        </v-btn>
      </v-form>

      <v-container>
        <v-textarea
          name="input-7-1"
          label="Expecting style"
          v-model="convertedJson"
        ></v-textarea>
      </v-container>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "Home",

  components: {},
  data: () => ({
    convertedJson: "Expecting Json.....",
    reader: new FileReader(),
  }),
  methods: {
    csvToArray(str, delimiter = ",") {
      console.log(str, "csvToArray");
      // slice from start of text to the first \n index
      // use split to create an array from string by delimiter
      const headers = str.slice(0, str.indexOf("\r")).split(delimiter);
      // slice from \n index + 1 to the end of the text
      // use split to create an array of each csv value row
      const strArr = str.slice(str.indexOf("\n") + 1).split("\n");

      // filterng the arrays to get rid of strings with "/r" character
      let filteredArray = strArr.map((item) => {
        return item.includes("\r") ? item.replace("\r", "") : item;
      });
      // Map the rows
      // split values from each row into an array
      // use headers.reduce to create an object
      // object properties derived from headers:values
      // the object passed as an element of the array
      const arr = filteredArray.map((row) => {
        const values = row.split(delimiter);
        const el = headers.reduce((object, header, index) => {
          if (values.length == 1) {
            return {};
          }
          object[header] = values[index];
          return object;
        }, {});
        return el;
      });

      // return the array
      return arr;
    },
    submitFile(e) {
      e.preventDefault();
      const csvFile = document.querySelector("#file");
      const input = csvFile.files[0];

      this.reader.onload = (e) => {
        const text = e.target.result;
        console.log(text);
        const data = this.csvToArray(text);
        this.convertedJson = JSON.stringify(data);
      };

      this.reader.readAsText(input);
    },
  },
};
</script>
