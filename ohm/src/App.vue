<template>
  <v-app>
    <v-stepper v-model="e1">
      <v-stepper-header>
        <v-stepper-step :complete="e1 > 1" step="1" editable>Productos</v-stepper-step>
  
        <v-divider></v-divider>
  
        <v-stepper-step :complete="e1 > 2" step="2" editable>Otros productos</v-stepper-step>
  
        <v-divider></v-divider>
  
        <v-stepper-step step="3" editable>Envío y vigencia</v-stepper-step>

        <v-divider></v-divider>
  
        <v-stepper-step step="4" editable>Pago</v-stepper-step>

        <v-divider></v-divider>
  
        <v-stepper-step step="5" editable>Vista previa</v-stepper-step>

        <v-divider></v-divider>
  
        <v-stepper-step step="6" editable>Enviar</v-stepper-step>
      </v-stepper-header>
  
      <v-stepper-items>
        <v-stepper-content step="1">
          <v-layout wrap>
                      <v-flex xs8>
              <v-autocomplete
                v-model="product"
                :disabled="isUpdating"
                :items="shop"
                filled
                chips
                color="blue-grey lighten-2"
                label="Select"
                item-text="name"
                item-value="name"
              >
                <template v-slot:selection="data">
                  <v-chip
                    v-bind="data.attrs"
                    :input-value="data.selected"
                    @click="data.select"
                  >
                    <v-avatar left>
                      <v-img :src="data.item.img"></v-img>
                    </v-avatar>
                    {{ data.item.name }}
                  </v-chip>
                </template>
                <template v-slot:item="data">
                  <template v-if="typeof data.item !== 'object'">
                    <v-list-item-content v-text="data.item"></v-list-item-content>
                  </template>
                  <template v-else>
                    <v-list-item-avatar>
                      <img :src="data.item.img">
                    </v-list-item-avatar>
                    <v-list-item-content>
                      <v-list-item-title v-html="data.item.name"></v-list-item-title>
                      <v-list-item-subtitle v-html="`$${data.item.price}`"></v-list-item-subtitle>
                    </v-list-item-content>
                  </template>
                </template>
              </v-autocomplete>
            </v-flex>
            <v-flex xs2>
              <label class="l5" for="quantity">Cantidad</label>
              <v-text-field
                v-model="quantity"
                type="number"
                value="0"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs2>
              <v-btn class="mx-2" fab dark color="indigo" @click="create()">
                <v-icon dark>add</v-icon>
              </v-btn>
            </v-flex>
          </v-layout>
          
          
          <div class="amount"> 
            
            <!--<a href="#" class="l6"><i class="fas fa-plus"></i></a>-->
          </div>
          <!--NODOS PARA CREAR LA TABLA???-->
          <!--tabla donde se van a imprimir los productos que vaya agregando-->
          <v-simple-table
            :fixed-header="fixedHeader"
            :height="height"
          >
            <thead>
              <tr>
                <th class="text-left">Producto</th>
                <th class="text-left">Cantidad</th>
                <th>Precio</th>
                <th>Editar</th>
                <th>Eliminar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in products" :key="item.name">
                <td>
                  <img :src="item.product.img" alt="">
                  {{ item.product.name }}
                </td>
                <td>{{ item.quantity }}</td>
                <td>{{ item.product.price }}</td>
                <td>ícono editar</td>
                <td>ícono eliminar</td>
              </tr>
            </tbody>
          </v-simple-table>
           <v-btn
            color="primary"
            @click="e1 = 2"
          >
            Continue
          </v-btn>
          <v-btn text>Cancel</v-btn>
        </v-stepper-content>
  
        <v-stepper-content step="2">
          <v-layout wrap>
            <v-flex xs6>
              <label class="l5" for="quantity">Nuevo producto</label>
              <v-text-field
                v-model="new_product.name"
                type="text"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs2>
              <label class="l5" for="quantity">Cantidad</label>
              <v-text-field
                v-model="new_product.quantity"
                type="number"
                value="0"
                min="0"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs2>
              <label class="l5" for="quantity">Precio</label>
              <v-text-field
                v-model="new_product.price"
                type="number"
                value="0"
                min="0"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs2>
              <v-btn class="mx-2" fab dark color="indigo" @click="create_new_product()">
                <v-icon dark>add</v-icon>
              </v-btn>
            </v-flex>
          </v-layout>
          <v-simple-table
            :fixed-header="fixedHeader"
            :height="height"
          >
            <thead>
              <tr>
                <th class="text-left">Producto</th>
                <th class="text-left">Cantidad</th>
                <th>Precio</th>
                <th>Editar</th>
                <th>Eliminar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in new_products" :key="item.name">
                <td>
                  {{ item.name }}
                </td>
                <td>{{ item.quantity }}</td>
                <td>{{ item.price }}</td>
                <td>ícono editar</td>
                <td>ícono eliminar</td>
              </tr>
            </tbody>
          </v-simple-table>
          <v-btn
            color="primary"
            @click="e1 = 3"
          >
            Continue
          </v-btn>
          <v-btn text>Cancel</v-btn>
        </v-stepper-content>
  
        <v-stepper-content step="3">
          <section class="sending-validity">
          <h1>COTIZADOR</h1>
            <div class="val">
              <label class="l9" for="validity">Vigencia</label>
              <input class="i9" type="text" id="validity">
            </div> 
            <div class="sending">
              <p>Envio a domicilio</p>
              <input class="i10" type="radio" value="Sin envío" id="no-sending">
              <label class="l10" for="no-sending">Sin envío</label>
              <input class="i11" type="radio" value="Con envío" id="sending">
              <label class="l11" for="send">Con envío</label>
            </div>
            <div class="postal-code">
              <label class="l12" for="CP">Código Postal</label>
              <input class="i12" type="number" id="CP">
              <input class="i13" type="submit" value="Generar costo">
              <p id="print-cost">$</p>
            </div>

            </section>

          <v-btn
            color="primary"
            @click="e1 = 4"
          >
            Continue
          </v-btn>
          <v-btn text>Cancel</v-btn>
        </v-stepper-content>
  
        <v-stepper-content step="4">
          <div class="payment">
            <p>Forma de pago</p>
            <input class="i14" type="radio" value="Efectivo" id="cash">
            <label class="l13" for="cash">Efectivo</label>

            <input class="i15" type="radio" value="Tarjeta" id="card">
            <label class="l14" for="card">Tarjeta</label>

            <input class="i16" type="radio" value="Credito" id="credit">
            <label class="l15" for="credit">Crédito</label>
          </div>
          <div class="currency">
            <p>Divisa</p>

            <input class="i17" type="radio" value="US" id="dollar">
            <label class="l16" for="dollar">US</label>

            <input class="i18" type="radio" value="MNX" id="pesos">
            <label  class="l17" for="pesos">MNX</label>

            <p id="exchange"></p>
          </div>
                
          <input class="i19" type="submit" value="Cotizar">
          
          <v-btn
            color="primary"
            @click="e1 = 5"
          >
            Continue
          </v-btn>
          <v-btn text>Cancel</v-btn>
        </v-stepper-content>
          
          
        <v-stepper-content step="5">
          <section class="preview">
            <h1>COTIZADOR</h1>
            <h2>RESUMEN</h2>
            <table>
              <thead>
                <tr>
                  <th>Cant</th>
                  <th>Producto</th>
                  <th>Precio Unitario</th>
                  <th>Total</th>
                </tr>
              </thead>
                <tr>
                <td>Celda cantidad</td>
                  <td>Celda producto</td>
                  <td>Celda precio unitario</td>
                  <td>Celda total</td>
                </tr>
            </table>

            
          </section>

            <section class="preview-costs">
              <h1>COTIZADOR</h1>

              <p>Subtotal $</p>
              <p id="preview-subtotal"></p>

              <p>IVA $</p>
              <p id="IVA"></p>

              <p>Envío $</p>
              <p id="preview-sending"></p>

              <p>TOTAL $</p>
              <p id="preview-total"></p>

              <p id="monthly">"aquí se imprime la mensualidad"</p>

              <p>"aquí se imprime la vigencia"</p>
              <p>"aquí se imprime el precio del dolar"</p>

              <input class="i20" type="submit" value="Editar">
              <input class="i20" type="submit" value="Guardar">
              <input class="i20" type="submit" value="Enviar">
              <input class="i20" type="submit" value="Cancelar">
            </section>
            <v-btn
              color="primary"
              @click="e1 = 6"
            >
              Vista Previa 
            </v-btn>
            <v-btn text>Cancel</v-btn>
        </v-stepper-content>
      </v-stepper-items>
    </v-stepper>

    <v-content>
      <!-- <HelloWorld/> -->
      <Dashboard/> 
    </v-content>
  </v-app>
</template>

<script>
//import HelloWorld from './components/HelloWorld';
import Dashboard from './components/Dashboard';

export default {
  name: 'App',
  components: {
    //HelloWorld,
    Dashboard
  },
  data () {
    return {
      e1: 0,
      dense: false,
      fixedHeader: true,
      height: 300,
      product: '',
      new_product: {
        name: '',
        quantity: '',
        price: ''
      },
      quantity: '',
      new_products: [],
      products: [],
      isUpdating: false,
      shop: [
        {
          name: 'Solenoide Tipo Chapa 12V',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/1512animation_400x308.gif?v=1526736322',
          price: 515.00
        }, {
          name: 'Solenoide 5V ',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/solenoidemini01_450x450.jpeg?v=1526736317',
          price: 195.00
        }, {
          name:'Filamento PLA Premium Gris 1.75mm 1kg',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/PLA-grey-filamento-colorplus-premium_333x324.jpg?v=1529186506',
          price: 515.00
        }, {
          name: 'Filamento ABS Premium Negro 1.75mm 1kg ',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/Caja-para-WebABSBLACKpsd-uai-423x423_423x423.jpg?v=1529186529',
          price: 515
        }, {
          name: 'Sonoff RF - WiFi Wireless Smart Switch with RF ',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/Captura_de_pantalla_2017-02-02_16.04.56_401x401.png?v=1526736325',
          price: 500
        }, {
          name:'Lámpara LED WiFi de Techo BN-SZ01 - Sonoff ',
          img:'https://cdn.shopify.com/s/files/1/1040/8806/products/Captura_de_pantalla_2017-04-21_13.05.11_395x388.png?v=1529186524',
          price: 295
        }, {
          name: 'Kit Robot Móvil',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/robot_movil_2_1408x1408.jpg?v=1552670758',
          price: 1100.00
        }, {
          name: 'IBM TJBot – A Watson Maker Kit',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/14515-04_600x600.jpg?v=1529186478',
          price: 3720.00
        },
        {
          name: 'Bote de Tinta Conductiva 50ml',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/10994-01b_450x450.jpeg?v=1526735663',
          price: 930.00
        }, {
          name: 'Alambre Magneto de Cobre - 11 metros / 0.1mm 38AWG',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/3522-03_970x728.jpg?v=1529186481',
          price: 40.00
        },
        {
          name: 'NodeMCU ESP8266 ESP-12 WiFi IoT',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/ESP8266LUA_4_1336x1064.jpg?v=1533573778',
          price: 195.00
        },
        {
          name: 'Domótica con Home Assistant',
          img: 'https://cdn.shopify.com/s/files/1/1040/8806/products/Domotica_con_Home_Assistant_-_Imagen_b20a8359-66b4-4aa7-9a36-734b76cdbdd3_1408x1408.png?v=1562351553',
          price: 3450.00
        },
      ]
    }
  },
  watch: {
    isUpdating (val) {
      if (val) {
        setTimeout(() => (this.isUpdating = false), 3000)
      }
    },
  },

  methods: {
    create: function() {
      let product_filter = this.shop.filter(product => product.name == this.product)[0]
      this.products.push({
        product: product_filter,
        quantity: this.quantity
      })
      this.clear_inputs()
    },
    create_new_product: function() {
      this.new_products.push(this.new_product)
      this.clear_inputs()
    },
    clear_inputs: function () {
      this.product = ''
      this.quantity = ''
    },
  },
};
</script>
    
<style scoped>
.l5 {
  color: black;
  font-size: 1rem;
}
</style>

