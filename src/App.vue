<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col>
          <b-jumbotron header="ISIC Explanation with ABELE">
            <hr/>
            <div class="double-column mt-4">
              <p>
                Skin cancer is the most common cancer globally, with melanoma being the most deadly form.
                Dermoscopy is a skin imaging modality that has demonstrated improvement for diagnosis of
                skin
                cancer compared to unaided visual inspection. However, clinicians should receive adequate
                training for those improvements to be realized. In order to make expertise more widely
                available, opaque artificial intelligence models need to go along with detailed and
                trustworthy
                explanations.
              </p>
              <p>
                This web application shows the result of an AI-based system that suggests a classification
                for a
                dermoscopic image. Beside the outcome of the system, we present a series of images that
                serve as
                explanation for the decision of the AI System: a counter example image, that shows an
                example
                that the AI system classifies with a different label; and a set of 4 exemplars showing
                images
                with the same classification as the original one.
              </p>
            </div>
          </b-jumbotron>
        </b-col>
      </b-row>
      <b-row class="mt-5">
        <b-col>
          <p>
            ⮕ <em>Choose one of the case studies we selected using the menu below</em>
          </p>
          <b-form-select v-model="current_explanation_id"
                         :options="explanations"
                         value-field="id"
                         id="selettore"></b-form-select>
        </b-col>
      </b-row>
      <b-row class="mt-5">
        <b-col>
          <h4>Image to explain (predicted class)</h4>
          <b-card no-body
                  class="overflow-hidden"
                  style="max-width: 540px;">
            <b-row no-gutters>
              <b-col md="6">
                <b-card-img
                    :src="`${publicPath}explanations/Ex_${current_explanation_id}/img_to_explain_${current_explanation_id}.png`"
                    alt="Image to explain"
                    class="rounded-0"></b-card-img>
              </b-col>
              <b-col md="6">
                <b-card-body>
                  <b-card-text>
                    <h3>
                      <b-badge>{{ getClassFromId(current_explanation.class) }}</b-badge>
                    </h3>
                    <div><strong>Neighborhood:</strong>
                      <ul>
                        <li v-for="e in Object.entries(current_explanation.neighborhood)"
                            :key="e[0]">
                          {{ getClassFromId(e[0]) }}: {{ e[1] }}
                        </li>
                      </ul>
                    </div>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
        <b-col>
          <h4>Counter example image (class)</h4>
          <b-card no-body
                  class="overflow-hidden"
                  style="max-width: 540px;">
            <b-row no-gutters>
              <b-col md="6">
                <b-card-img :src="`${publicPath}explanations/Ex_${current_explanation_id}/counter_1.png`"
                            alt="Counter example"
                            class="rounded-0"></b-card-img>
              </b-col>
              <b-col md="6">
                <b-card-body>
                  <b-card-text>
                    <h3>
                      <b-badge>
                        {{ getClassFromId(current_explanation.counter_class) }}
                      </b-badge>
                    </h3>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>
      <b-row class="mt-5">
        <b-col>
          <h4>Exemplar images</h4>
          <p>The following images are generated syntethically and they are classified with class
            <b>{{ getClassFromId(current_explanation.class) }}</b>
            by the blackbox.</p>
        </b-col>
      </b-row>
      <b-row class="mb-5">
        <b-col v-for="i in [1,2,3,4]"
               :key="i">
          <b-img thumbnail
                 fluid
                 :src="`${publicPath}explanations/Ex_${current_explanation_id}/proto_${i}.png`"
                 :alt="`Exemplar Image ${i}`"></b-img>
        </b-col>
      </b-row>
      <div class="border-top p-5">
        <b-row class="mb-3">
          <b-col>
            <h5 class="text-uppercase">Skin cancer descriptions</h5>
          </b-col>
        </b-row>
        <b-row>
          <b-col class="double-column">
            <p><strong>Melanoma</strong>: is a type of skin cancer taht developes from the pigment-producing cells known
              as
              melanocytes. Often Melanoma developes from moles: Changes in a mole that can indicate melanoma
              include an increase in size, irregular edges, change in color, itchiness, or skin breakdown.</p>
            <p><strong>Melanocytic nevus</strong>: is a type of melanocytic tumor that contains nevus cells. The most
              common
              types of moles are skin tags, raised moles and flat moles. Benign moles are usually brown, tan,
              pink or black (especially on dark-colored skin). They are circular or oval and are usually small
              (commonly between 1–3 mm), though some can be larger than the size of a typical pencil eraser
              (>5 mm).</p>

            <p><strong>Basal cell carcinoma</strong>: is the most common type of skin cancer. It often appears as a
              painless
              raised area of skin, which may be shiny with small blood vessels running over it. Basal-cell
              carcinoma typically present with a shiny, pearly skin nodule.</p>

            <p><strong>Actinic keratosis</strong>: is a pre-cancerous area of thick, scaly, or crusty skin. Size
              commonly
              ranges
              between 2 and 6 millimeteres, but they can grow to be several centimeters in diameter. Notably,
              AKs are often felt before they are seen, and the texture is sometimes compared to sandpaper.
              They may be dark, light, tan, pink, red, a combination of all these, or have the same color as
              the surrounding skin.</p>

            <p><strong>Dermatofibroma</strong>: is a benign skin growth. Dermatofibromas are hard solitary slow-growing
              papules
              that may appear in a variety of colours, usually brownish to tan; they are often elevated or
              pedunculated.</p>

            <p><strong>Vascular lesion</strong>: is any medical condition that affects the integumentary system - the
              organ
              system that encloses the body and includes the skin, hair, nails, and related muscle and glands.
              They are relatively common abnormalities of the skin and underlying tissues, more commonly known
              as birthmarks.</p>
          </b-col>
        </b-row>
      </div>

    </b-container>
    <footer class="mt-5 p-5 border-top bg-light">
      <b-container>
        <b-row class="mb-3"
               align-h="center">
          <b-col class="text-right border-right">
            <img class="mr-4"
                 :src="`${publicPath}img/xai_logo_color@2x.png`"
                 alt=""
                 height="55px">
            <img class="mr-4"
                 :src="`${publicPath}img/logo_ai4eu.png`"
                 alt=""
                 height="45px">
          </b-col>
          <b-col>
            <p>This project is funded by the European Projects XAI (Grant agreement ID: 834756) and AI4EU (Grant
              agreement ID: 825619)</p>
          </b-col>
        </b-row>
      </b-container>
    </footer>
  </div>
</template>


<script>
const d3 = require('d3');


export default {
  name: 'App',
  data() {
    return {
      explanations: [],
      classes: {},
      current_explanation_id: null,
    }
  },
  mounted() {
    d3.csv(this.publicPath + 'classes.csv')
        .then(res => {
          const cls = {};
          res.forEach(r => {
            cls[r.code] = r;
          })
          this.classes = cls;
        });

    d3.json(this.publicPath + 'explanations/instances.json')
        .then(res => {
          this.explanations = res.map(e => ({
            ...e,
            text: `id:${e.id} - class:${this.getClassFromId(e.class)}`
          }));
          this.current_explanation_id = res[0].id;
        })
  },
  computed: {
    publicPath: function () {
      return process.env.BASE_URL || '/';
    },
    current_explanation: function () {
      const id = this.current_explanation_id;
      const sel = this.explanations.find(e => e.id == id);

      return sel;
    }
  },
  methods: {
    getClassFromId: function (cid) {
      return this.classes[cid].name;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}

@media (min-width: 992px) {
  .double-column {
    column-count: 2;
    column-gap: 2.5rem;
  }
}

#selettore {
  font-weight: bold;
}

ul {
  list-style-position: inside;
  padding-left: 0;
}
</style>
