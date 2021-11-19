<template>
  <div>
    <v-stepper v-model="e1">
      <v-stepper-header>
        <template v-for="n in steps">
          <v-stepper-step
            :key="`${n}-step`"
            :complete="e1 > n"
            :step="n"
            color="#ed1c27"
          >
            Paso {{ n }}
          </v-stepper-step>

          <v-divider v-if="n !== steps" :key="n"></v-divider>
        </template>
      </v-stepper-header>

      <v-stepper-items>
        <v-stepper-content v-for="n in steps" :key="`${n}-content`" :step="n">
          <v-card
            class="mb-12"
            color="lighten-1"
            height="500px"
            style="overflow-y: auto;"
          >
            <v-row dense v-if="n == 2" class="p-30">
              <h2 class="ml-2">Producto Seleccionado:</h2>
              <v-col cols="12">
                <v-card :color="'white'" class="pb-5">
                  <div class="d-flex flex-no-wrap justify-space-between">
                    <div>
                      <v-card-title
                        class="text-h5"
                        v-text="product.name.toUpperCase()"
                      >
                      </v-card-title>
                      <h4 class="ml-4">Estado: {{ product.status }}</h4>
                      <h4 class="ml-4">Tipo: {{ product.type }}</h4>
                      <h4 class="ml-4">Numero: {{ product.number }}</h4>
                      <v-chip class="ma-2" color="#ed1c27" text-color="white">
                        Balance: {{ formatCurrency("es-CO", "COP", 2, product.balance)}} COP
                      </v-chip>
                    </div>
                  </div>

                  
                </v-card>
              </v-col>
               <v-col cols="12" class="center">
                   <v-btn
                          color="grey"
                          style="color:white"
                          @click="nextStep(2)"
                        >
                          Cancelar
                        </v-btn>
               </v-col>
            </v-row>

            <v-row dense v-if="n == 1" class="p-30">
              <h2 class="ml-2">Seleccionar Un Producto:</h2>
              <v-col v-for="(item, i) in items" :key="i" cols="12">
                <v-card :color="item.status === 'activa' ? 'white' : '#d1d1d1'">
                  <div
                    class="d-flex flex-no-wrap justify-space-between"
                    style="border-color: rgb(0 0 0);"
                  >
                    <div>
                      <v-card-title
                        class="text-h5"
                        v-text="item.name.toUpperCase()"
                      ></v-card-title>
                      <v-card-subtitle
                        v-text="item.status.toUpperCase()"
                      ></v-card-subtitle>

                      <v-card-actions>
                        <v-btn
                          v-if="item.status === 'activa'"
                          color="#ed1c27"
                          style="color:white"
                          @click="selectProduct(item)"
                        >
                          Seleccionar
                        </v-btn>
                      </v-card-actions>
                    </div>
                  </div>
                </v-card>
              </v-col>
            </v-row>
          </v-card>
        </v-stepper-content>
      </v-stepper-items>
    </v-stepper>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  mounted () {
    this.getProductsByClient()
  },
  name: 'Products',
  data: () => ({
    items: [],
    e1: 1,
    steps: 2,
    product: {
      balance: '',
      name: '',
      number: '',
      status: '',
      type: ''
    }
  }),
  watch: {
    steps (val) {
      if (this.e1 > val) {
        this.e1 = val
      }
    }
  },
  methods: {
    nextStep (n) {
      if (n === this.steps) {
        this.e1 = 1
      } else {
        this.e1 = n + 1
      }
    },
    selectProduct (product) {
      console.log(product)
      this.product = product
      this.nextStep(1)
    },
    getProductsByClient (clientId = 'b4b9ca80-f41c-4eb1-89a8-fd2c8bf11bc7') {
      axios.get('http://localhost:2222/api/products/' + clientId).then(res => {
        this.items = res.data
      })
    },
    formatCurrency (locales, currency, fractionDigits, number) {
      let formatted = new Intl.NumberFormat(locales, {
        style: 'currency',
        currency: currency,
        minimumFractionDigits: fractionDigits
      }).format(number)
      return formatted
    }
  }
}
</script>
