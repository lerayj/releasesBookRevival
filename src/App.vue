<template>
  <v-app top-navbar>
    <header>
      <v-toolbar>
        <v-toolbar-logo>Release Book</v-toolbar-logo>
      </v-toolbar>
    </header>
    <main>
      <v-content>
        <div v-if="loader">
          <v-progress-circular indeterminate v-bind:size="70" v-bind:width="7" class="purple--text"/>
        </div>
        <v-container v-else fluid>
          <v-tabs id="mobile-tabs-3" grow>
            <v-tab-item 
              v-for="(item, i) in products"
              v-bind:href="'#mobile-tabs-3-' + i"
              slot="activators"
            >
              {{ item.slug }}
            </v-tab-item>
            <v-tab-content 
              v-for="(item, i) in products" 
              v-bind:id="'mobile-tabs-3-' + i"
              slot="content"
            >
              <v-card>
                <v-card-text>TUTU</v-card-text>
              </v-card>
            </v-tab-content>
          </v-tabs>
        </v-container>
      </v-content>
    </main>
  </v-app>
</template>

<script>
  import * as firebase from 'firebase';

  var config = {
      apiKey: "AIzaSyBmIXXwQgWlg-ToWJ7gYg-SFdJBejczK4M",
      authDomain: "releasesbook.firebaseapp.com",
      databaseURL: "https://releasesbook.firebaseio.com",
      storageBucket: "releasesbook.appspot.com",
      messagingSenderId: "406872894854"
  };
  var app = firebase.initializeApp(config);

  export default {
    data () {
      return {
        products: undefined,
        releases: undefined
      }
    },
    computed: {
      loader: function(){
        console.log("this.products: ", this.products, "this.releases: ", this.releases);
        return !this.products && !this.releases;
      }
    },
    created: function(){
      this.loadProducts();
      this.loadReleases();
    },
    methods: {
      loadReleases: function(){
        var self = this;
        return firebase.database().ref('/releases/').once('value').then(function(snapshot) {
          console.log("releases: ", snapshot.val());
          self.releases = snapshot.val();
          return snapshot.val();
        });
      },
      loadProducts: function(){
        var self = this;
        return firebase.database().ref('/products/').once('value').then(function(snapshot) {
          console.log("products: ", snapshot.val());
          self.products = snapshot.val();
          return snapshot.val();
        });
    }
  }
}

</script>

<style lang="stylus">
  @import '../node_modules/vuetify/src/stylus/main'
  @import './css/main.css'
</style>