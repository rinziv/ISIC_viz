<template>
  <div id="app">
    <b-container>
      <b-row>
        <h1>ISIC Explanation with ABELE</h1>
        <hr/>
      </b-row>
      <b-row>
        <b-col>
          <b-form-select v-model="current_explanation_id" :options="explanations"
                         text-field="id" value-field="id"></b-form-select>
        </b-col>
      </b-row>
      <b-row class="mt-5">
        <b-col>
          <b-card no-body class="overflow-hidden" style="max-width: 540px;">
            <b-row no-gutters>
              <b-col md="6">
                <b-card-img :src="`explanations/Ex_${current_explanation_id}/img_to_explain_${current_explanation_id}.png`" alt="Image to explain" class="rounded-0"></b-card-img>
              </b-col>
              <b-col md="6">
                <b-card-body title="Image to explain">
                  <b-card-text>
                    <ul>
                      <li><b>id:</b>{{current_explanation.id}}</li>
                      <li><b>Predicted class:</b>{{current_explanation.class}}</li>
                      <li><b>Neighborhood:</b> {{current_explanation.neighborhood}}</li>
                    </ul>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
        <b-col>
          <b-card no-body class="overflow-hidden" style="max-width: 540px;">
            <b-row no-gutters>
              <b-col md="6">
                <b-card-img :src="`explanations/Ex_${current_explanation_id}/counter_1.png`" alt="Counter example" class="rounded-0"></b-card-img>
              </b-col>
              <b-col md="6">
                <b-card-body title="Counter example">
                  <b-card-text>
                    <ul>
                      <li><b>Counter example class:</b>{{current_explanation.counter_class}}</li>
                    </ul>
                  </b-card-text>
                </b-card-body>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>
      <b-row class="mt-4">
        <b-col>
          <h2>Prototype images</h2>
          <p>The following images are generated syntethically and they are classified with class <b>{{current_explanation.class}}</b> by the blackbox.</p>
        </b-col>
      </b-row>
      <b-row>
        <b-col v-for="i in [1,2,3,4]" :key="i">
          <b-img thumbnail fluid :src="`explanations/Ex_${current_explanation_id}/proto_${i}.png`" :alt="`Prototype Image ${i}`"></b-img>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
const d3 = require('d3');


export default {
  name: 'App',
  data(){
    return {
      explanations: [],
      classes: {},
      current_explanation_id: null,
    }
  },
  mounted(){
    d3.csv('classes.csv')
    .then(res => {
      const cls = {};
      res.forEach(r => {
        cls[r.code] = r;
      })
      this.classes = cls;
    });

    d3.json('explanations/instances.json')
      .then(res => {
        this.explanations = res;
        this.current_explanation_id = res[0].id;
      })
  },
  computed:{
    current_explanation: function(){
      const id = this.current_explanation_id;
      const sel = this.explanations.find(e => e.id == id);

      return sel;
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
</style>
