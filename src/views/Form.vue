<template>
  <v-container>
    <div class="d-flex">
      <v-btn depressed color="primary" @click="openHome()"> Back </v-btn>
    </div>

    <v-form>
      <h1 class="d-flex justify-center">Form Customer</h1>
      <v-row>
        <v-col cols="12">
          <v-text-field
            v-model="form.fullname"
            label="Fullname"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12">
          <v-text-field
            v-model="form.phone_number"
            label="Phone Numberr"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" class="d-flex justify-center">
          <v-btn color="success" class="mr-4" @click="submit()"> Submit </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      form: {
        fullname: "",
        phone_number: "",
      },
    };
  },
  methods: {
    openHome() {
      this.$router.push({ path: "/" });
    },
    submit() {
      if (this.$route.params.id) {
        axios
          .put(`http://localhost:3000/customer/${this.$route.params.id}`, this.form)
          .then((response) => {
            console.log(response);
            this.$router.push({ path: "/" });
          })
          .catch((e) => {
            this.errors.push(e);
          });
      } else {
        axios
          .post(`http://localhost:3000/customer`, this.form)
          .then((response) => {
            console.log(response);
            this.$router.push({ path: "/" });
          })
          .catch((e) => {
            this.errors.push(e);
          });
      }
    },
  },
  mounted() {
    console.log(this.$route.params.id);
    if (this.$route.params.id != null) {
      axios
        .get(`http://localhost:3000/customer/${this.$route.params.id}`)
        .then((response) => {
          this.form = response.data.data;
        })
        .catch((e) => {
          this.errors.push(e);
        });
    }
  },
};
</script>