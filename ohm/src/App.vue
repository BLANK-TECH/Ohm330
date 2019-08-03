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
                      <v-img src="data.item.img"></v-img>
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
            <v-flex xs12>
              <label class="l5" for="quantity">Vigencia de la cotización</label>
              <div>
                <v-text-field
                  v-model="validity"
                  type="number"
                  value="0"
                  min="0"
                >
                </v-text-field>
                días
              </div>
            </v-flex>
            <div class="sending">
              <p>Envio a domicilio</p>
              <label class="l10" for="no-sending">Sin envío</label>
              <v-switch v-model="shipping" inset></v-switch>
              <label class="l11" for="send">Con envío</label>
            </div>
            <div class="postal-code" v-if="shipping">
              <label class="l12" for="CP">Código Postal</label>
              <v-text-field
                v-model="zone_code"
                type="number"
                value="0"
                min="0"
              >
              </v-text-field>
              <v-btn class="ma-2" color="primary" dark @click="generate_cost()">
                Consultar
                <v-icon dark right>local_shipping</v-icon>
              </v-btn>
              <p id="print-cost" v-if="shipping_cost > 0">${{shipping_cost}}</p>
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
      validity: 0,
      height: 300,
      validity: 0,
      zone_code: 0,
      shipping_cost:0,
      product: '',
      new_product: {
        name: '',
        quantity: 0,
        price: 0
      },
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
    generate_cost: function () {
      if (this.zone_code >= 1000 || this.zone_code <= 19999) {
        this.shipping_cost = 156.99;
      } 
      if (this.zone_code >= 50000 || this.zone_code <= 52497) {
        this.shipping_cost = 176.85;
      } else {
        this.shipping_cost = "No value found";
      }
    }
  },
};
</script>
    
<style scoped>
.l5 {
  colorscpink;
*{
    margin: 0%;
    padding: 0%;
    overflow-x: hidden;
}



header {
    width: 100vw;
    height: 10vh;
    border-bottom: 4px solid rgb(250, 137, 67);   
    box-shadow:  0px 2px 10px grey;
    
}   

header .logo {
    width: 40%;
    height: 100%;
    display: inline-block;
}

header .logo img {
    width: 100%;
    height: 70%;
    margin: 5% 0;
}

header .icon-menu {
    width: 40%;
    height: 100%;
    display: inline-block;
    float: right;
   
}

header .icon-menu a {
    width: 40%;
    height: 100%;
    float: right;
    margin: 0 5% 0 0;
     overflow-y: hidden;
}


header .icon-menu a i {
    width: 60%;
    height: 80%;
    float: right;
    margin: 40% 5% 0 0;
    color: black;
    font-size: 1.5rem;
}


header .menu {
    display: none;
}

.sign-in {
    width: 100vw;
    height: 90vh;
}

.sign-in form {
    width: 80%;
    height: 80%;
    margin: 25% auto;
}

.sign-in form .l1 {
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1.5rem;
    margin: 0 15%;
}

.sign-in form .i1[type=text] {
    display: inline-block;
    width: 70%;
    height: 10%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1.5rem;
    margin: 2% 0 10% 15%;
} 

.sign-in form .l2 {
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1.5rem;
    margin: 0 15%;
}
.sign-in form .i2[type=password]{
    display: inline-block;
    width: 70%;
    height: 10%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1.5rem;
    margin: 2% 0 10% 15%;
}


.sign-in form .i3[type=submit]{
    display: inline-block;
    width: 70%;
    height: 10%;
    display: block;
    color: #fcfcfc;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1.5rem;
    margin: 20% 0 20% 15%;
}

.sign-in form a {
    text-decoration: none;
    color:#262625;
    margin: 0 25%;
}


.quote-search {
    width: 100vw;
    height: 100vh;
    
    position: relative;
}

.quote-search h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}

.quote-search .product {
    display: inline-block;
    width: 40%;
    height: 10%;
}

.quote-search .product .l4 { 
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}

.quote-search .product .i4[type=search] {
    display: inline-block;
    width: 90%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.quote-search .amount {
    display: inline-block;
    float:  right;
    width: 55%;
    height: 10%;
}

.quote-search .amount .l5 { 
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}

.quote-search .amount .i5[type=number] {
    display: inline-block;
    width: 25%;
    height: 38%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
  
}


/*no se está acomodando 
.quote-search .amount a {
    display: inline-block;
    float: right;
    background-color: blue;
}

.quote-search .amount a i{
    display: inline-block;
    float: right;
}
*/

.quote-search .arrowA #arrow2{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    right: 10%;
}

.quote-search .arrowB #arrow3{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    left: 10%;
}


.other-products {
    width: 100vw;
    height: 100vh;
    position: relative;
}

.other-products h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}

.other-products .other {
        display: inline-block;
        width: 40%;
        height: 10%;
}

.other-products .other .l6 {
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}

.other-products .other .i6[type=text] {
    display: inline-block;
    width: 90%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.other-products .other-item-quantity {
    display: inline-block;
    width: 25%;
    height: 10%;
}

.other-products .other-item-quantity .l7{
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}

.other-products .other-item-quantity .i7[type=number] {
    display: inline-block;
    width: 80%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.other-products .other-item-price {
    display: inline-block;
    width: 25%;
    height: 10%;
}

.other-products .other-item-price .l8{
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}

.other-products .other-item-price .i8[type=number] {
    display: inline-block;
    width: 80%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.other-products  .arrowC #arrow4{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    right: 10%;
}


.other-products  .arrowD #arrow5{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    left: 10%;
}

.sending-validity {
    width: 100vw;
    height: 100vh;
    position: relative;
}

.sending-validity h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}
.sending-validity .val {
    display: inline-block;
    width: 100%;
    height: 10%;
}
.sending-validity .l9 {
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}

.sending-validity .i9[type=text] {
    display: inline-block;
    width: 30%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.sending-validity .sending {
        display: block;
        width: 100%;
        height: 10%;
}

.sending-validity .sending p {
    display: block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 3% 8%;
}

.sending-validity .sending .i10, .l10, .i11, .l11{
    display: inline-block;
    margin: 0 0 0 8%;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
}
.sending-validity .sending .l10, .i11, .l11{
    margin: 0;
}
.sending-validity .postal-code {
    display: block;
    width: 100%;
    height: 25%;
    margin: 5% 0;
}
.sending-validity .postal-code .l12,.i12, .i13, p {
    display: block;
    margin: 3% 9%;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
}

.sending-validity .postal-code .i12[type=number] {
    display: inline-block;
    width: 30%;
    height: 15%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 9%;
}

.sending-validity .postal-code .i13[type=submit]{
    display: inline-block;
    width: 50%;
    height: 20%;
    display: block;
    color: #fcfcfc;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 8% auto 3% auto;
  
}
.sending-validity .arrowE #arrow6{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    right: 10%;
}
.sending-validity .arrowF #arrow7{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    left: 10%;
}

.payment-methods {
    width: 100vw;
    height: 100vh;
    position: relative;
}

.payment-methods h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}

.payment-methods .payment {
    width: 100%;
    height: 20%;
 
}

.payment-methods .payment .i14, .l13, .i15, .l14, .i16, .l15{
    display: inline-block;
    margin: 0 0 0 8%;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
}

.payment-methods .payment .l13, .i15, .l14, .i16, .l15 {
    margin: 0 2%;
}

.payment-methods .currency {
    width: 100%;
    height: 20%;
    margin: 5% 0;
    
}

.payment-methods .currency .i17, .l16, .i18, .l17 {
    display: inline-block;
    margin: 0 0 0 8%;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
}

.payment-methods .payment  .l16, .i18, .l17 {
    margin: 0 2%;
}

.payment-methods .i19[type=submit] {
    display: inline-block;
    width: 50%;
    height: 5%;
    display: block;
    color: #fcfcfc;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 5% auto 3% auto;
  
}

.payment-methods .arrowG #arrow8{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    left: 10%;
}

.preview {
    width: 100vw;
    height: 100vh;
    position: relative;
}

.preview h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}

.preview h2 {
    display: inline-block;
    margin: 0 0 0 8%;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 10% 8%;
}

.preview .arrowH #arrow9{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    right: 10%;
}

.preview .arrowI #arrow10{ 
    width: 10%;
    height: 5%;
    font-size: 2rem;
    color: black;
    position: absolute;
    bottom: 10%;
    left: 10%;
}

.preview-costs {
    width: 100vw;
    height: 100vh;
    position: relative;
}

.preview-costs h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}


.preview-costs .i20[type=submit] {
    display: inline-block;
    width: 50%;
    height: 5%;
    display: block;
    color: #fcfcfc;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 5% auto 3% auto;
  
}

.email{
    width: 100vw;
    height: 100vh;
    position: relative;
}

.email h1 {
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    font-size: 2.5rem;
    margin: 12vh 0 0 0;
    text-align: center;
    box-shadow:  0px 2px 10px grey;
}

.email .email1 {
    display: inline-block;
    width: 50%;
    height: 10%;
    margin: 5% 0 0 8%;
    
}

.email .email1 .l17 {
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}


.email .email1  .i21[type=text] {
    display: inline-block;
    width: 80%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.email .email2 {
    display: block;
    width: 50%;
    height: 10%;
    margin: 5% 0 0 8%;
}

.email .email2 .l18 {
    display: inline-block;
    color: #999490;
    font-weight: bolder;
    font-size: 1rem;
    margin: 3% 5% 0 8%;
}


.email .email2 .i22[type=email] {
    display: inline-block;
    width: 80%;
    height: 45%;
    display: block;
    color: black;
    background-color: #f8f4f1;
    border-radius: 5px;
    border: 3px solid #FF6300;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 0 0 0 8%;
}

.email  .i23[type=submit] {
    display: inline-block;
    width: 50%;
    height: 5%;
    display: block;
    color: #fcfcfc;
    background: linear-gradient(110deg, #FF6300 60%, rgb(250, 137, 67) 60%);
    border-radius: 5px;
    box-shadow: 0px 2px 10px grey;
    text-align: center;
    font-size: 1rem;
    margin: 10% auto 3% auto;
}
