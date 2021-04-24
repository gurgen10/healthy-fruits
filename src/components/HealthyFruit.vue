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
              healthytips.com
            </v-btn>
          </h3>
          <h3 class="form-title">
            <b>Enter the details of the article you want to order</b>
          </h3>
          <v-container>
            <v-row>
              <v-col cols="12" sm="12" md="12">
                <v-text-field label="Source" v-model="source"></v-text-field>
              </v-col>
              <v-col cols="12" sm="12" md="12">
                <v-text-field label="Instractions" v-model="instractions"></v-text-field>
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
                  ref="budget"
                  v-model="price"
                  prefix="$"
                ></v-text-field>
              </v-col>
              <v-col>
                <v-radio-group 
                  v-model="extraSelectedPrice"
                  column
                  @change="changeExtraPrice">
                  <template v-slot:label>
                    <div><b>Please, select options (optional)</b></div>
                  </template>
                  <v-radio
                    label="Bilingual (add 20%)"
                    value="20%"
                  ></v-radio>
                  <v-radio
                    label="Proof-reading (add 10%)"
                    value="10%"
                  ></v-radio>
                  <v-radio
                    label="Copy-editing ($ 20)"
                    value="20$"
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
            Submit $ {{totalAmount}}
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
    extraSelectedPrice: '',
    price: 35,
    extraPrice: 0
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
    price: {
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
    totalPrice()  {
      return this.price + this.extraPrice
    },
    totalAmount()  {
      return this.selectedCount * this.totalPrice
    },
  },

  methods: {
    changeExtraPrice(e) {
      if(e === '20%') {
        this.extraPrice = this.price * 0.2
      } else if (e === '10%') {
        this.extraPrice = this.price * 0.1
      } else {
        this.extraPrice = 20
      }
    },
    
    submitOrder() {
    

      this.shwowDialog = false
    }
  }
};
</script>

<style scoped>
.form-title {
  color: blue;
}
</style>
