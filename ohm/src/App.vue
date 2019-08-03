<template>
  <v-app>
    <v-stepper v-model="e1">
      <v-stepper-header>
        <v-stepper-step :complete="e1 > 1" step="1" editable>Productos</v-stepper-step>
  
        <v-divider></v-divider>
  
        <v-stepper-step :complete="e1 > 2" step="2" editable>Otros productos</v-stepper-step>
  
        <v-divider></v-divider>
  
        <v-stepper-step :complete="e1 > 3" step="3" editable>Envío y vigencia</v-stepper-step>

        <v-divider></v-divider>
  
        <v-stepper-step :complete="e1 > 4" step="4" editable>Pago</v-stepper-step>

        <v-divider></v-divider>
  
        <v-stepper-step :complete="e1 > 5" step="5" editable>Vista previa</v-stepper-step>

        <v-divider></v-divider>
  
        <v-stepper-step :complete="e1 > 6" step="6" editable>Enviar</v-stepper-step>
      </v-stepper-header>
  
      <v-stepper-items>
        <v-stepper-content step="1">
          <v-layout wrap>
                      <v-flex xs6>
              <v-autocomplete
                class="busqueda"
                v-model="product"
                :disabled="isUpdating"
                :items="shop"
                filled
                chips
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
            <v-flex xs3>
              <label class="cantidad-texto" for="quantity">Cantidad</label>
              <v-text-field class="cantidad-input"
                v-model="quantity"
                type="number"
                value="0"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs3>
              <v-btn class="add"  @click="create()">
                <v-icon dark>+</v-icon>
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
                <th>Producto</th>
                <th>Cantidad</th>
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
            class="add1"
            @click="e1 = 2"
          >
            Continue
          </v-btn>
          <v-btn class="add1" text>Cancel</v-btn>
        </v-stepper-content>
  
        <v-stepper-content step="2">
          <v-layout wrap>
            <v-flex xs4>
              <label  class="cantidad-texto" for="quantity">Nuevo producto</label>
              <v-text-field
                class="cantidad-input"
                v-model="new_product_name"
                type="text"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs3>
              <label class="cantidad-texto" for="quantity">Cantidad</label>
              <v-text-field
              class="cantidad-input"
                v-model="new_product_quantity"
                type="number"
                value="0"
                min="0"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs2>
              <label class="cantidad-texto" for="quantity">$</label>
              <v-text-field
                class="cantidad-input"
                v-model="new_product_price"
                type="number"
                value="0"
                min="0"
              >
              </v-text-field>
            </v-flex>
            <v-flex xs2>
              <v-btn class="add"  @click="create_new_product()">
                <v-icon dark>+</v-icon>
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
            class="add1"
            @click="e1 = 3"
          >
            Continue
          </v-btn>
          <v-btn class="add1" text>Cancel</v-btn>
        </v-stepper-content>
  
        <v-stepper-content step="3">
          <section class="sending-validity">
            <v-flex xs10>
              <label class="cantidad-texto" for="quantity">Vigencia de la cotización</label>
              <div>
                <v-text-field
                  class="cantidad-input1"
                  v-model="validity"
                  type="number"
                  value="0"
                  min="0"
                >
                </v-text-field>
                <span class="cantidad-texto">días</span>
              </div>
            </v-flex>

            <div class="sending">
              <p class="cantidad-texto">Envio a domicilio</p>
              <p class="cantidad-texto">Sin envío</p>
              <v-switch v-model="shipping" inset></v-switch>
              <p class="cantidad-texto">Con envío</p>
            </div>
            <div v-if="shipping">
              
              <v-text-field
                class="cantidad-input2"
                v-model="zone_code"
                type="number"
                value="0"
                min="0"
                
              >
              </v-text-field>
              
              <v-btn class="consultar" @click="generate_cost()">
                Consultar
                <v-icon dark right></v-icon>
              </v-btn>
              <p v-if="shipping_cost > 0">${{shipping_cost}}</p>
            </div>

            </section>

          <v-btn
            class="add1"
            @click="e1 = 4"
          >
            Continue
          </v-btn>
          <v-btn class="add1" text>Cancel</v-btn>
        </v-stepper-content>
  
        <v-stepper-content step="4">
          <div class="payment">
            <p class="cantidad-texto">Forma de pago</p>
            <v-radio-group v-model="payment">
              <v-radio
                label="Efectivo"
                color="orange darken-3"
                value="effective"
              ></v-radio>
              <v-radio
                label="Tarjeta"
                color="orange darken-3"
                value="card"
              ></v-radio>
              <v-radio
                label="Crédito"
                color="orange darken-3"
                value="credit"
              ></v-radio>
            </v-radio-group>
            <div v-if="payment == 'credit'">
              <div>
                <label class="cantidad-texto">Mensualidades</label>
                <v-text-field
                class="cantidad-input3"
                  v-model="month_pay"
                  type="number"
                  value="0"
                  min="0"
                >
                </v-text-field>
              </div>
              <div>
                <label>Intereses</label>
                <v-text-field
                  class="cantidad-input3"
                  v-model="interest"
                  type="number"
                  value="0"
                  min="0"
                >
                </v-text-field>
                <span>%</span>
              </div>
            </div>
          </div>
          <div class="currency">
            <p>Divisa</p>
            <v-radio-group v-model="currency">
              <v-radio
                label="MXN"
                color="orange darken-3"
                value="peso"
              ></v-radio>
              <v-radio
                label="USD"
                color="orange darken-3"
                value="dollar"
              ></v-radio>
            </v-radio-group>
          </div>
          <div v-if="currency == 'dollar'">
            <v-btn class="consultar" @click="generate_currency()">
              Consultar
              <v-icon dark right></v-icon>
            </v-btn>
            <p>{{current_currency}}</p>
          </div>          
          <v-btn
            class="add1"
            @click="e1 = 5"
          >
            Continue
          </v-btn>
          <v-btn  class="add1" text>Cancel</v-btn>
        </v-stepper-content>
          
          
        <v-stepper-content step="5">
          <section class="preview">
            <h1>Cotización</h1>
            <p>
              No. de cotización: {{id_quotation}}
            </p>
            <h2>RESUMEN</h2>
            <table >
              <thead>
                  <tr>
                      <th>
                        Cantidad
                      </th>
                      <th id="stud" scope="col">
                        Descripción
                      </th>
                      <th id="chal" scope="col">
                          Precio
                      </th>
                      <th id="villa" scope="col">
                          Importe
                      </th>
                  </tr>
              </thead>
              <tbody>
                  <tr v-if="products.length > 0">
                      <th id="par" class="span" colspan="5" scope="colgroup">
                        En inventario
                      </th>
                  </tr>
                  <tr v-for="item in products" :key="item.name">
                      <th headers="par" id="pbed1">
                        {{item.quantity}}
                      </th>
                      <td headers="par pbed1 stud">
                        {{item.product.name}}
                      </td>
                      <td headers="par pbed1 chal">
                          ${{item.product.price}}
                      </td>
                      <td headers="par pbed1 villa">
                          ${{item.product.price*item.quantity}}
                      </td>
                  </tr>
                  <tr v-if="new_products.length > 0">
                      <th id="rome" class="span" colspan="5" scope="colgroup">
                        Nuevo producto
                      </th>
                  </tr>
                  <tr v-for="item in new_products" :key="item.name">
                      <th headers="par" id="pbed1">
                        {{item.quantity}}
                      </th>
                      <td headers="par pbed1 stud">
                        {{item.name}}
                      </td>
                      <td headers="par pbed1 chal">
                          ${{item.price}}
                      </td>
                      <td headers="par pbed1 villa">
                          ${{item.price*item.quantity}}
                      </td>
                  </tr>
              </tbody>
            </table>

              <p>Subtotal $</p>
              <p id="preview-subtotal">{{subtotal}}</p>

              <p>IVA $</p>
              <p id="IVA">{{iva}}</p>

              <p v-if="shipping">Envío $</p>
              <p v-if="shipping" id="preview-sending">{{shipping_cost}}</p>

              <p v-if="payment == 'credit'">
                Mensualidades: {{month_pay}} <br>
                Interés: {{interest}}% <br>
                Total: ${{total*100/interest}} <br>
                A pagar en cada mensualidad: ${{(total*100/interest)/month_pay}}
              </p>
              <p v-else id="preview-total">TOTAL ${{total}}</p>


              <p>Vigencia de la cotización: {{validity}}</p>
              <p v-if="currency == 'dollar'">{{current_currency}}</p>

          </section>
            <v-btn
               class="add1"
              @click="e1 = 6"
            >
              Vista Previa 
            </v-btn>
            <v-btn  class="add1" text>Cancel</v-btn>
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
      currency: 'peso',
      current_currency: '',
      height: 300,
      validity: 0,
      zone_code: 0,
      shipping_cost:0,
      payment: 'effective',
      interest: 0,
      month_pay: 0,
      product: '',
      new_product_quantity: 0,
      new_product_price: 0,
      new_product_name: '',
      quantity: 0,
      new_products: [],
      products: [],
      isUpdating: false,
      shipping: false,
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

  computed: {
    id_quotation() {
      return Math.round(Date.now() + Math.random())
    },
    subtotal(){
      let total_product = []
      let sum_stock = this.products.map(item => total_product.push(item.product.price*item.quantity))
      let total_new_product = []
      let sum_new = this.new_products.map(item => total_new_product.push(item.price*item.quantity))
      
      return total_product.reduce((a, b) => a + b, 0) + total_new_product.reduce((a, b) => a + b, 0)
    },
    iva() {
      return this.subtotal*0.16
    },
    total() {
      return this.subtotal + this.iva + this.shipping_cost
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
      this.new_products.push({
        name: this.new_product_name,
        quantity: this.new_product_quantity,
        price: this.new_product_price
      })
      this.clear_inputs_new()
    },
    clear_inputs_new: function() {
      this.new_product_name = '',
      this.new_product_quantity = 0,
      this.new_product_price = 0
    },
    clear_inputs: function () {
      this.product = ''
      this.quantity = ''
    },
    generate_cost: function () {
      if (this.zone_code >= 1000 || this.zone_code <= 19999) {
        this.shipping_cost = 156.99;
      } 
      if (this.zone_code >= 50000 || this.zone_code <= 52497) {
        this.shipping_cost = 176.85;
      } else {
        this.shipping_cost = "No value found";
      }
    },
    generate_currency: function() {
      let today = new Date();
      let dd = String(today.getDate()).padStart(2, '0');
      let mm = String(today.getMonth() + 1).padStart(2, '0'); //January is 0!
      let yyyy = today.getFullYear();

      today = mm + '/' + dd + '/' + yyyy;
      this.current_currency = `Precio del dolar el día ${today}:  19.31`
    }
  },
};
</script>
    
<style scoped>


.busqueda{
    display: inline;
    width: 80%;
    height: 55%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 15% 0 0 8%;
    font-family: ‘Cabin’, sans-serif;
}

 .cantidad-texto { 
    display: inline;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 0% 0 8%;
    font-family: ‘Cabin’, sans-serif;
    
}
.cantidad-input {
    display: inline;
    width: 90%;
    height: 40%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 ;
    font-family: ‘Cabin’, sans-serif;
}

.add {
    display: inline;
    width: 20%;
    height: 60%;
    display: block;
    color: black;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 10% 0 0 0%;
    font-family: ‘Cabin’, sans-serif;
}

.consultar {
  display: inline;
    width: 60%;
    height: 60%;
    display: block;
    color: black;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 10% 0 0 0%;
    font-family: ‘Cabin’, sans-serif;
}

.cantidad-input1 {
    display: inline;
    width: 70%;
    height: 5%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 5% 0;
    font-family: ‘Cabin’, sans-serif;
}

.cantidad-input2 {
    display: inline;
    width: 60%;
    height: 5%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 5% 0;
    font-family: ‘Cabin’, sans-serif;
}

.add1 {
    width: 45%;
    height: 60%;
    display: inline-block;
    color: black;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 10% 5% 0 0%;
    font-family: ‘Cabin’, sans-serif;
}

.cantidad-input3 {
    display: inline;
    width: 60%;
    height: 10%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 5% 0;
    font-family: ‘Cabin’, sans-serif;
}
.preview {
    display: inline;
    color:  #3d3d3c ;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 0% 0 8%;
    font-family: ‘Cabin’, sans-serif;
}

</style>