<template>
  <div class="product">
    <top-header @header_message="option = $event" class="mb-5"></top-header>
    <v-container class="grey lighten-5">
      <v-row
        no-gutters
        class="mb-4"
      >
        <v-col cols = "4">
         <v-card
          class="mx-auto my-12"
          max-width="374"
        >
         <v-img
            max-height="300"
            contain
            class="grey lighten-4"
            :src="this.$store.state.current_product.photo"
          ></v-img>

          <v-card-title>
            {{this.$store.state.current_product.name.charAt(0).toUpperCase() +
            this.$store.state.current_product.name.slice(1)}}
          </v-card-title>

          <v-card-text>
            <v-row
              align="center"
              class="mx-0"
            >
              <v-rating
                :value="this.$store.state.current_product.score"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
              ></v-rating>

              <div class="grey--text ml-4">
                {{this.$store.state.current_product.score}}  ({{$store.state.product_reviews.length}})
              </div>
            </v-row>

            <div class="my-4 subtitle-1">
              <b>Marca:</b> {{this.$store.state.current_product.brand}}
            </div>

            <div class="my-4 subtitle-1">
              <b>Precio:</b> ${{ this.$store.state.current_product.price }}
            </div>

            <div class="my-4 subtitle-1">
              <b>Fecha de creación:</b> {{ this.$store.state.current_product.created_at.substring(0, 10) }}
            </div>
            
            <div style="text-align: justify;">
              <b>Descripción:</b> {{this.$store.state.current_product.description.charAt(0).toUpperCase() +
              this.$store.state.current_product.description.slice(1)}}
            </div>
          </v-card-text>

          <v-divider class="mx-4"></v-divider>

          <v-card-text>
            <v-row class="mb-2" justify=center>
                <v-dialog
                  v-model="dialog"
                  persistent
                  max-width="50%"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      color="#102f85"
                      dark
                      v-bind="attrs"
                      v-on="on"
                    >
                      Crear review
                    </v-btn>
                  </template>
                  <v-card v-if="$store.state.logged">
                    <v-card-title>
                      <span class="text-h5">Crear una crítica</span>
                    </v-card-title>
                    <v-card-text>
                      <v-container>
                        <v-row>
                           <v-col
                            cols="12"
                          >
                            <v-textarea
                              outlined
                              name="comentar"
                              label="Comentario"
                              counter = 400
                              v-model="comment"
                              :rules="[v => (v || '' ).length <= 400 || 'El comentario debe ser de 400 comentario o menos.']"
                            ></v-textarea>
                          </v-col>
                          <v-col cols=12>
                            <v-rating
                              v-model="rating"
                              color="yellow darken-3"
                              background-color="grey darken-1"
                              empty-icon="$ratingFull"
                              half-increments
                              hover
                              large
                            ></v-rating>
                          </v-col>
                        </v-row>
                      </v-container>
                      <small>*Por favor rellena todos los campos</small>
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="red"
                        text
                        @click="dialog = false; comment='', rating=4.5"
                      >
                        Cerrar
                      </v-btn>
                      <v-btn
                        color="#102f85"
                        text
                        @click="save"
                      >
                        Guardar
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                  <v-card v-else>
                    <v-card-title class="text-h5">
                      ¿No puedes realizar una crítica?
                    </v-card-title>
                    <v-card-text>
                      Los usuarios que no han iniciado sesión no pueden realizar críticas sobre los productos. 
                      Si aún no tienes una cuenta puedes registrarte y empezar a comentar!
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="green darken-1"
                        text
                        @click="gotoSignin"
                      >
                        Registrarme
                      </v-btn>
                      <v-btn
                        color="green darken-1"
                        text
                        @click="gotoLoggin"
                      >
                        Iniciar sesión
                      </v-btn>
                      <v-btn
                        color="red"
                        text
                        @click="dialog = false"
                      >
                        Cerrar
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
            </v-row>
          </v-card-text>
        </v-card>
        </v-col>
        <v-col cols = "8">
          <v-card
            class="mx-auto my-12"
            color="#102f85"
            dark
            max-width="80%"
            v-for="(review, index) in $store.state.product_reviews"
            :key="index"
          >
            <v-card-title>
              <v-icon
                large
                left
              >
                mdi-check-circle
              </v-icon>
              <span class="title font-weight-light">ChooseMe</span>
            </v-card-title>

            <v-card-text>
              <v-row class="mx-3 my-1">
                <div class="font-weight-bold" style="text-align: justify;font-size: 120%;">"{{review.comment}}"</div>
              </v-row>
              <v-row class="ml-3 mt-5">
                <div>Calificación: ({{review.score}})
                  <v-tooltip right>
                    <template v-slot:activator="{ on, attrs }">
                      <v-icon v-bind="attrs" v-on="on" medium>mdi-information</v-icon>
                    </template>
                    <span>Las calificaciones de las críticas estan en una escala de 1-5.</span>
                  </v-tooltip>
                </div>
                </v-row>
                <v-row class="ml-3">
                  <div>Creado: {{review.created_at.substring(0, 10)}}</div>
                </v-row>
            </v-card-text>

            <v-card-actions>
              <v-list-item class="grow">
                <v-list-item-avatar color="grey darken-3">
                  <v-img
                    class="elevation-6"
                    alt=""
                    src="@/assets/avatar.png"
                  ></v-img>
                </v-list-item-avatar>

                <v-list-item-content>
                  <v-list-item-title>{{review.user_name}}</v-list-item-title>
                </v-list-item-content>

                <v-row
                  align="center"
                  justify="end"
                >
                  <v-btn icon @click="reaction(review,1)">
                    <v-icon class="mr-2">
                      mdi-thumb-up
                    </v-icon>
                  </v-btn>
                  <span class="subheading mr-2">{{review.ups}}</span>
                  <span class="mr-1">·</span>
                  <v-btn icon @click="reaction(review,-1)">
                    <v-icon class="mr-2">
                      mdi-thumb-down
                    </v-icon>
                  </v-btn>
                  <span class="subheading">{{review.downs}}</span>
                </v-row>
              </v-list-item>
            </v-card-actions>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
              color="indigo darken-2"
              dark
              :disabled = "review.impressions.length == 0"
              @click="ConsultaActualizaciones(review)"
              >
                  Consultar actualizaciones
              </v-btn>
            </v-card-actions>
          </v-card>
          <infinite-loading @infinite="getReviews" class="mt-5">
            <div slot="waveDots">
                            <v-alert
                            elevation="4"
                            color="#283593"
                            dense
                            type="info"
                            >
                                <strong> Cargando... </strong>
                            </v-alert>
                        </div>
                        <div slot="no-more">
                            <v-alert
                            elevation="4"
                            color="#283593"
                            dense
                            type="info"
                            >
                                <strong> No hay más resultados </strong>
                            </v-alert>
                        </div>
                        <div slot="no-results">
                            <v-alert
                            elevation="4"
                            color="#283593"
                            dense
                            type="info"
                            >
                                <strong> No hay más resultados </strong>
                            </v-alert>
                        </div>
          </infinite-loading>
        </v-col>
      </v-row>
    </v-container>

    <v-snackbar
      v-model="snackbar"
      :multi-line="multiLine"
      >
        {{ message }}

        <template v-slot:action="{ attrs }">
          <v-btn
            :color="color"
            text
            v-bind="attrs"
            @click="snackbar = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>

      <v-dialog
            v-model="botonActualizaciones"
            persistent
            max-width="80%"
      >
        <v-card>
            <v-list two-line>
            <template v-for="(review) in this.reviewActualizaciones">
                <v-divider
                :key="review.impression_id"
                >
                </v-divider>
                <v-list-item
                :key="review.impression_id"
                >
                <v-list-item-content>
                    <v-list-item-title v-html="review.impression"></v-list-item-title>
                    <v-list-item-subtitle>Creado el: {{review.created_at.substring(0, 10)}} </v-list-item-subtitle>
                </v-list-item-content>
                </v-list-item>
            </template>
            </v-list>
        </v-card>

        <v-btn
            color="error"
            @click="botonActualizaciones = false"
        >
            Cerrar
        </v-btn>
       </v-dialog>

  </div>
</template>

<script>
import axios from "axios";
import TopHeader from "../components/TopHeader";
import InfiniteLoading from "vue-infinite-loading";
export default {
  data (){
    return {
      dialog: false,
      comment: "",
      rating: 4.5,
      snackbar: false,
      message: "No puedes crear más de una review sobre el mismo producto",
      color: "",
      multiLine: true,
      reviewActualizaciones: [],
      botonActualizaciones: false
    }
  },
  components: {
    "top-header": TopHeader,
    InfiniteLoading,
  },
  methods: {
    async getReviews($state) {
      this.$store.commit("incrementPage_product_reviews");
      try {
        const response = await axios.get(
          "http://localhost:8080/review/"+this.$store.getters.getCurrent_product.product_id+"/"+this.$store.getters.getPage_product_reviews,
          {}
        );
        if (response.data.length == 0) { //No hay más resultados
          $state.complete();
        } else {
          this.$store.commit("setProduct_reviews", response.data);
          $state.loaded();
        }
      } catch (error) {
        console.log(error);
      }      
    },
    gotoLoggin(){
        this.dialog = false;
        this.$router.push("/login").catch(() => {});
        window.scrollTo(0, 0);
    },
    gotoSignin(){
      this.dialog = false;
      this.$router.push("/signin").catch(() => {});
      window.scrollTo(0, 0);
    },
    async reaction(review, option) {
      if (!this.$store.state.logged) {
        this.message = "Para reaccionar a los comentarios debes tener una cuenta!";
        this.color = "red";
        this.snackbar = true;
        return
      }
      try {
        const response = await axios.post("http://localhost:8080/users/like",
        {
          "comment_id": review.comment_id,
          "up_down": option
        },
        {
            headers: { Authorization: "Bearer " + localStorage.getItem("token") },
        });
        if ((response.data == "Nuevo like") && (option == 1)) {
          review.ups = review.ups + 1;
        } else if ((response.data == "Nuevo dislike") && (option == -1)) {
          review.downs = review.downs + 1;
        } else if ((response.data == "Ya le había dado like") && (option == 1)) {
          review.ups = review.ups - 1;
        } else if ((response.data == "Ya le había dado like") && (option == -1)) {
          review.ups = review.ups - 1;
          review.downs = review.downs + 1;
        } else if ((response.data == "Ya le había dado dislike") && (option == 1)) {
          review.ups = review.ups + 1;
          review.downs = review.downs - 1;
        } else if ((response.data == "Ya le había dado dislike") && (option == -1)) {
          review.downs = review.downs - 1;
        }
        this.message = "Reacción guardada!";
        this.color = "green";
        this.snackbar = true;
      } catch (error) {
          console.log(error);
      }
    },
    async save(){
      if (this.comment.length > 400){
        this.message = "La extensión del comentario no puede exceder los 400 caracteres";
        this.color = "red";
        this.snackbar = true;
        return
      }
      this.loading = true;
       try {
        const response1 = await axios.post(
          "http://localhost:8080/product/newreview",
          {
            comment: this.comment,
            score: this.rating,
            product_id: this.$store.state.current_product.product_id
          },
          {
            headers: { Authorization: "Bearer " + localStorage.getItem("token") },
          }
        );
        if (!response1.data) {
          this.message = "No puedes crear más de una review sobre el mismo producto";
          this.color = "red";
          this.snackbar = true;
          this.comment = "";
          this.rating = 4.5;
          this.dialog = false;
        } else {
          try {
            const response = await axios.get(
              "http://localhost:8080/review/" + this.$store.state.current_product.product_id + "/0",
              {}
            );
            this.$store.commit("resetProduct_reviews", response.data);
            this.$store.commit("resetPage_product_reviews");
          } catch (error) {
            console.log(error);
          }
          try {
            const response = await axios.get(
              "http://localhost:8080/product/" + this.$store.state.current_product.product_id,
              {}
            );
            this.$store.commit("setCurrent_product", response.data);
          } catch (error) {
            console.log(error);
          }
          this.message = "Review creada!";
          this.color = "green";
          this.snackbar = true;
        }
        this.dialog = false;
      } catch (error) {
        console.log(error);
      }
    },
    ConsultaActualizaciones(review){
        this.reviewActualizaciones = review.impressions;
        this.botonActualizaciones = true;
    },
  }
};
</script>
