<template>
  <v-row justify="center">
    <v-dialog v-model="shwowDialog" persistent max-width="600px">
      <template v-slot:activator="{ on, attrs }">
        <v-btn color="primary" dark v-bind="attrs" v-on="on">
          Create New Content
        </v-btn>
      </template>
      <v-card>
        <v-img height="200px" :src="require('@/assets/img/healthyFruit.jpg')">
          <v-app-bar flat color="rgba(0, 0, 0, 0)">
            <v-btn color="white" fab dark small @click="shwowDialog = false">
              <h3><v-icon color="black">mdi-arrow-left</v-icon></h3>
            </v-btn>
            <v-spacer></v-spacer>
          </v-app-bar>
        </v-img>
        <v-card-title>
          <h3>10 health fruits You should eat</h3>
        </v-card-title>
        <v-form @submit.prevent="submitOrder">
          <v-card-text>
          <h3>
            <v-btn color="blue" link href="#" small text>
              {{order.publication}}
            </v-btn>
          </h3>
          <h3 class="form-title">
            <b>{{order.description}}</b>
          </h3>
          <v-container>
            <v-row>
              <v-col cols="12" sm="12" md="12">
                <v-text-field 
                  label="Source"
                  :error-messages="sourceErrors"
                  v-model.trim="source"
                  ></v-text-field>
              </v-col>
              <v-col cols="12" sm="12" md="12">
                <v-text-field
                label="Instractions"
                :error-messages="instractionsErrors"
                v-model="instractions"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6">
                <v-select 
                  :items="countList" 
                  label="Number of Writers"
                  v-model="selectedCount"
                  required></v-select>
              </v-col>
              <v-col cols="12" sm="6"> 
                <v-text-field
                  label="Budget (USD)"
                  :error-messages="budgetErrors"
                  ref="budget"
                  v-model="budget"
                  prefix="$"
                ></v-text-field>
              </v-col>
              <v-col>
                <v-radio-group 
                  v-model="extraSelectedBudget"
                  column
                  @change="changeExtraBudget">
                  <template v-slot:label>
                    <div><b>Please, select options (optional)</b></div>
                  </template>
                  <v-radio
                    v-for="option in order.options"
                    :key="`${option.extra_id}`"
                    :id="`${option.id}`"
                    :label="`${option.name} (add ${option.increase? option.increase + '%' : option.price + '$'})`"
                    :value="`${option.increase? option.increase + '%' : option.price + '$'}`"
                  ></v-radio>
                </v-radio-group>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions class="justify-center">
          <v-btn
            type="submit"
            tile
            color="primary"
          >
            <v-icon left>
              mdi-basket
            </v-icon>
            Submit $ {{totalAmount?  totalAmount :  ''}}
          </v-btn>
        </v-card-actions>
        </v-form>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import { required, maxLength, url, numeric, between } from 'vuelidate/lib/validators';
export default {
  props: {
    order: {
      type: Object, 
      required: true
    }
  },
  data: () => ({
    shwowDialog: false,
    source: '',
    instractions:'',
    selectedCount: 1,
    extraSelectedBudget: '',
    budget: 0,
    extraBudget: 0
  }),
  validations: {
    source: {
      required,
      url,
    },
    instractions:{
      required,
      maxLength: maxLength(30)
    },
    budget: {
      required,
      numeric,
      between: between(5,500)
    },
  },
  computed: {
    countList: () => {
      const list = [];
      for (let i = 1; i <= 15; i += 1) {
        list.push(i);
      }
      return list;
    },
    totalBudget()  {
      return this.selectedCount * this.budget
    },
    totalAmount()  {
      return this.totalBudget + this.extraBudget
    },
    sourceErrors () {
      const errors = [];
      if (!this.$v.source.$dirty) return errors;
      !this.$v.source.url && errors.push('Source must be valid url.');
      !this.$v.source.required && errors.push('Source is required.');
      return errors
    },
    instractionsErrors () {
      const errors = []
      if (!this.$v.instractions.$dirty) return errors;
      !this.$v.instractions.maxLength && errors.push(`Instractions must be ${this.$v.instractions.$params.maxLength.max} character length.`);
      !this.$v.instractions.required && errors.push('Instractions is required.');
      return errors
    },
    budgetErrors () {
      const errors = []
      if (!this.$v.budget.$dirty) return errors;
      !this.$v.budget.numeric && errors.push('Budget must be a number');
      !this.$v.budget.required && errors.push('Budget is required.');
      !this.$v.budget.between && errors.push(`Budget mast be between ${this.$v.budget.$params.between.min} and ${this.$v.budget.$params.between.max}.`);
      return errors;
    },
  },

  methods: {
    changeExtraBudget(e) {
      if(e === '20%') {
        this.extraBudget = this.totalBudget * 0.2;
      } else if (e === '10%') {
        this.extraBudget = this.totalBudget * 0.1;
      } else {
        this.extraBudget = 20;
      }
    },
    
    submitOrder() {
      if(this.$v.$invalid) {
        this.$v.$touch();
        return;
      }
      this.$v.$reset();
      this.source = '';
      this.instractions = '';
      this.budget = 0;
      this.selectedCount = 1;

      this.$emit('orderSubmited');
      this.shwowDialog = false;
    }
  }
};
</script>

<style scoped>
.form-title {
  color: blue;
}
</style>
