<template>
  <v-container>
    <v-app-bar app color="#131313" height="70" flat dark fixed style="position: fixed;">
      <div class="text-white ml-10">
        <v-img src="@/assets/logo1.png" width="150" height="150" class="rounded-xl" alt="logo"></v-img>
      </div>
      <v-spacer></v-spacer>
      <v-app-bar-nav-icon class="nav mr-6" color="white" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-btn class="menu mr-3 text-white" @click="scrollToSection(item.section)" text v-for="item in items"
        :key="item.title" :class="{ 'active': activePage === item.section }">
        {{ item.title }}
      </v-btn>
      <div class="text-white menu rounded mr-10" style="border: 2px solid #ac831c;">
        <v-btn class="menu" color="#ac831c">Create your card</v-btn>
        <hr style="display: inline; color: #ac831c;" class="ml-2">
        <v-btn class="menu" color="#ac831c" append-icon="mdi-login">Login</v-btn>
      </div>
    </v-app-bar>
    <v-navigation-drawer color="black" v-model="drawer" app class="position-fixed" location="right">
      <v-list dense fixed>
        <center class="mt-16">
          <v-list-item v-for="item in items" :key="item.title" link :exact="item.exact"
            @click="scrollToSection(item.section)" :class="{ 'active': activePage === item.section }"
            :style="{ backgroundColor: activePage === item.section ? '#FFC107' : '', color: activePage === item.section ? 'black' : '' }">
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="mb-4">{{ item.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-icon>mdi-bank</v-icon>
          <v-list-item-title class="mb-4">Create your card</v-list-item-title>
          <v-icon>mdi-login</v-icon>
          <v-list-item-title class="mb-4">Login</v-list-item-title>
        </center>
      </v-list>
    </v-navigation-drawer>
  </v-container>
  <v-container>
    <v-row>
      <v-col>
        <span class="text-h3">
          Create your <br>
          <span class="font-weight-bold">ZEEQR</span> card
        </span>
        <br><br>
        <span class="text-h5">Customize your card</span>
      </v-col>
      <v-col>
        <v-card :color="back === 'white' ? 'white' : 'black'" class="rounded-lg pa-5" width="340">
          <div class="d-flex flex-no-wrap justify-space-between">
            <div :style="{ color: textColor }">
              <v-card-actions>
                <div style="border: 2px solid rgb(128, 126, 126); width: 100px; border-radius: 10px;"
                  class="mt-10 pa-2 text-center">
                  <v-icon>mdi-wifi</v-icon>
                  <hr style="display: inline;" class="ml-2">
                  <v-icon class="ml-3">mdi-qrcode</v-icon>
                </div>
              </v-card-actions>
              <h3>{{ name }}</h3>
              <h5>{{ designation }}</h5>
            </div>
            <v-avatar class="ma-3 rounded-lg" size="125" rounded="0">
              <v-img src="@/assets/qr.jpg"></v-img>
            </v-avatar>
          </div>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <div class="text-subtitle-2">
        <span>Select your Card Material -</span>
        <v-btn v-for="option in cardMaterials" :key="option" v-model="cardMaterial" :value="option" variant="outlined"
          class="ml-2 text-subtitle-2" width="40"
          :style="{ backgroundColor: option === 'PVC' ? '#BDBDBD' : 'transparent' }">
          {{ option }}
        </v-btn><br><br>
        <span>Select your Card Color &nbsp; &nbsp; -</span>
        <v-btn v-for="option in cardColors" :key="option" v-model="cardColor" :value="option" variant="outlined"
          class="ml-2 text-subtitle-2" width="40" @click="back = option.toLowerCase()"
          :style="{ backgroundColor: option === 'Black' ? '#BDBDBD' : 'transparent' }">
          {{ option }}
        </v-btn><br><br>
        <span>Select your Text Color &nbsp; &nbsp; -</span>
        <v-btn v-for="option in textColors" :key="option" v-model="textColor" :value="option" variant="outlined"
          class="ml-2 text-subtitle-2" width="40" @click="textColor = option.toLowerCase()"
          :style="{ backgroundColor: option === 'White' ? '#BDBDBD' : 'transparent' }">
          {{ option }}
        </v-btn><br><br>
        <v-row>
          <v-col>
            <v-text-field maxlength="15" placeholder="Enter Your Name" label="Name display on card" v-model="name"
              variant="outlined" required></v-text-field>
          </v-col>
          <v-col>
            <v-text-field maxlength="20" placeholder="Enter Your Designation" label="Designation display on card"
              v-model="designation" variant="outlined" required></v-text-field>
          </v-col>
        </v-row>
        <v-col class="text-center">
          <v-btn color="black" @click="submit">Submit</v-btn>
        </v-col>
        <v-snackbar v-model="snackbar">
          <span class="text-green"> Successfully Submited...!</span>
          <template v-slot:actions>
            <v-btn color="yellow" variant="text" @click="snackbar = false">
              Close
            </v-btn>
          </template>
        </v-snackbar>
      </div>
    </v-row>
  </v-container>
</template>

<script>

export default {
  name: 'HelloWorld',

  data: () => ({
    drawer: false,
    back: '',
    textColor: 'white',
    activePage: 'home',
    cardMaterial: 'PVC',
    snackbar: false,
    name: 'Prajwal',
    designation: 'Frontend developer',
    items: [
      { title: 'Home', icon: 'mdi-home', section: 'home' },
      { title: 'About Us', icon: 'mdi-account', section: 'about' },
      { title: 'Contact', icon: 'mdi-phone', section: 'contact' },
      // { title: 'Create your card', icon: 'mdi-card', section: 'card' },
      // { title: 'Login', icon: 'mdi-login', section: 'login' },
    ],
    cardMaterials: ['PVC', 'Metal', 'Luxury'],
    cardColors: ['Black', 'White'],
    textColors: ['White', 'Gray', 'Gold', 'Silver', 'Foil'],
  }),
  methods: {
    scrollToSection(section) {
      this.activePage = section;
      this.$emit('scrollToSection', section);
    },
    submit() {
      const payload = {
        name: this.name,
        designation: this.designation
      }
      this.snackbar = true
      console.log(payload)
    }
  }
}
</script>

<style>
.active {
  text-decoration-line: underline;
  text-decoration-color: #FFC107;
  text-decoration-thickness: 3px;
  text-underline-offset: 10px;
}

.menu {
  text-transform: capitalize;
}

@media screen and (max-width: 800px) {
  .menu {
    display: none;
  }
}

@media screen and (min-width: 800px) {
  .nav {
    display: none;
  }
}
</style>