<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center">
      <v-col cols="5">
        <v-img
          :src="require('../assets/smiling-face-with-horns.png')"
          contain
          height="200"
        ></v-img>
      </v-col>
    </v-row>
    <v-row align="center" justify="center">
      <v-col cols="5">
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field
            v-model="target"
            label="Target Address"
            :rules="targetRules"
            required
          ></v-text-field>

          <v-text-field
            v-model="description"
            label="Short Description"
            :rules="descriptionRules"
            required
          ></v-text-field>

          <v-textarea
            name="input-7-1"
            v-model="hexData"
            filled
            label="Data (Hex Encoded)"
            :rules="hexDataRules"
            auto-grow
            required
            value=""
          ></v-textarea>

          <v-checkbox
            v-model="checkbox"
            label="I'm ok with this"
            required
          ></v-checkbox>

          <v-btn color="success" class="mr-4" @click="submit">
            Submit
          </v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  props: {
    overlay: String,
    minions: Array
  },
  components: {},
  data: () => ({
    valid: false,
    target: "",
    targetRules: [
      v => !!v || "Target is required",
      v =>
        (v && /^(0x){1}[0-9a-fA-F]{40}$/i.test(v)) || "Must be a valid address" // can use web3 checksum
    ],
    description: "",
    descriptionRules: [
      v => !!v || "Description is required",
      v => (v && v.length <= 200) || "Too Long"
    ],
    hexData: "",
    hexDataRules: [
      v => !!v || "Hex Data is required",
      v => (v && v.startsWith("0x")) || "Must start with 0x"
    ],
    checkbox: false
  }),
  methods: {
    submit() {
      if (!this.valid) {
        return false;
      }
      const minion = {};
      minion.name = "new minion";
      minion.target = this.target;
      minion.description = this.description;
      // TODO: might want to use proposalId for this
      minion.id = this.minions.length + 1;
      minion.hexData = this.hexData;
      minion.executed = false;
      this.$emit("submitted", minion);
    }
  }
};
</script>
