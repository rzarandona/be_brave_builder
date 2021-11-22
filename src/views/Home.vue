<template>
  <div>
    <NavigationBar />
    <transition name="fade">
      <Loader v-if="is_loading" />
    </transition>
    <main class="container">
      <div class="step">
        <p class="step-label">What's your email address?</p>
        <small class="notice">This email shall be used upon checkout.</small>
        <input class="character-name" type="text" v-model="purchaser_email" />
      </div>
      <transition name="fade">
        <div v-if="purchaser_email.length" class="step">
          <p class="step-label">Select a gender</p>
          <div class="gender-selector">
            <div
              @click="setCharacterGender('girl')"
              :class="{
                girl: true,
                'gender-item': true,
                active: character_gender == 'girl',
              }"
            >
              GIRL
            </div>
            <div
              @click="setCharacterGender('boy')"
              :class="{
                boy: true,
                'gender-item': true,
                active: character_gender == 'boy',
              }"
            >
              BOY
            </div>
          </div>
        </div>
      </transition>
      <transition name="fade">
        <div v-if="character_gender.length" class="step">
          <p class="step-label">Pick a character</p>
          <div v-if="character_gender == 'boy'" class="boy avatar-selection">
            <div
              @click="setSelectedAvatar(item)"
              v-for="item in boy_avatars"
              :class="{ 'avatar-item': true, active: character_avatar == item }"
            >
              <img
                :src="'https://bebrave.hectorspost.com/assets/' + item + '.jpg'"
                alt=""
              />
            </div>
          </div>
          <div v-if="character_gender == 'girl'" class="girl avatar-selection">
            <div
              @click="setSelectedAvatar(item)"
              v-for="item in girl_avatars"
              :class="{ 'avatar-item': true, active: character_avatar == item }"
            >
              <img
                :src="'https://bebrave.hectorspost.com/assets/' + item + '.jpg'"
                alt=""
              />
            </div>
          </div>
        </div>
      </transition>
      <transition name="fade">
        <div v-if="character_avatar.length" class="step">
          <p class="step-label">What's your child's name?</p>
          <input class="character-name" type="text" v-model="character_name" />
        </div>
      </transition>

      <transition name="fade">
        <div v-if="character_name.length" class="step">
          <p class="step-label">Cover Type</p>
          <div class="gender-selector">
            <div
              @click="setCoverType('softback')"
              :class="{
                softback: true,
                'cover-type-item': true,
                active: cover_type == 'softback',
              }"
            >
              SOFTBACK — €18
            </div>
            <div
              @click="setCoverType('hardback')"
              :class="{
                hardback: true,
                'cover-type-item': true,
                active: cover_type == 'hardback',
              }"
            >
              HARDBACK — €24
            </div>
          </div>
        </div>
      </transition>

      <button v-if="character_name.length" @click="getPreview" class="submit">
        GET PREVIEW
      </button>
    </main>
  </div>
</template>

<script>
import NavigationBar from "../components/NavigationBar.vue";
import Loader from "../components/Loader.vue";
import axios from "axios";

export default {
  data() {
    return {
      boy_avatars: ["boy-a", "boy-b", "boy-c", "boy-d", "boy-e"],
      girl_avatars: ["girl-a", "girl-b", "girl-c", "girl-d", "girl-e"],
      is_loading: false,
      character_avatar: "",
      character_gender: "",
      character_name: "",
      purchaser_email: "",
      cover_type: "",
    };
  },
  computed: {
    base_url() {
      return process.env.VUE_APP_BASE_URL;
    },
    s_pronoun() {
      if (this.character_gender == "girl") {
        return "she";
      }
      return "he";
    },
    o_pronoun() {
      if (this.character_gender == "girl") {
        return "her";
      }
      return "him";
    },
    p_pronoun() {
      if (this.character_gender == "girl") {
        return "her";
      }
      return "his";
    },
  },
  methods: {
    setSelectedAvatar(avatar) {
      this.character_avatar = avatar;
    },
    setCharacterGender(gender) {
      this.character_gender = gender;
    },
    setCoverType(cover_type) {
      this.cover_type = cover_type;
    },
    getPreview() {
      this.$store.state.purchaser_email = this.purchaser_email;
      this.$store.state.cover_type = this.cover_type;

      let instance = this;
      instance.is_loading = true;
      axios
        .get("https://bebraveapi.hectorspost.com/create.php", {
          params: {
            character_name: instance.character_name,
            gender: instance.character_gender,
            character: instance.character_avatar,
            s_pronoun: instance.s_pronoun,
            o_pronoun: instance.o_pronoun,
            p_pronoun: instance.p_pronoun,
            cover_type: instance.cover_type,
          },
        })
        .then((res) => {
          console.log(res.data);
          this.$store.state.preview_url = res.data.image;
          this.$store.state.inner_pdf_url = res.data.inner_pdf;
          this.$store.state.outer_pdf_url = res.data.outer_pdf;
          this.$store.state.source_id = res.data.source_id;

          instance.$router.push({
            name: "Preview",
          });
        })
        .catch((err) => {
          console.log(err);
          instance.is_loading = false;
        });
    },
  },
  components: {
    NavigationBar,
    Loader,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");
body,
body * {
  font-family: Poppins;
}

body {
  background: #fcf7ed;
  margin: 0;
}

.container {
  max-width: 1140px;
  margin: 0 auto;
}

main.container {
  padding: 20px 0;
  margin-bottom: 100px;
}

.step {
  background: white;
  padding: 20px;
  box-shadow: 0 10px 15px 0 rgba(0, 0, 0, 0.02);
  border-radius: 10px;
  padding: 50px;
  margin: 30px 0;
}
.step-label {
  color: black;
  font-size: 15px;
  margin: 0;
}

.gender-selector {
  display: grid;
  grid-template-columns: 1fr 1fr;
  margin-top: 20px;
}
.gender-item,
.cover-type-item {
  display: grid;
  place-content: center;
  padding: 30px;
  background: #f2f2f2;
  transition: 0.4s;
  font-size: 20px;
  cursor: pointer;
}
.gender-item.active,
.cover-type-item.active {
  background: #58a241;
  color: white;
}

.avatar-selection {
  margin-top: 50px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 30px;
}
.avatar-item {
  box-shadow: 0 0px 15px 0 rgba(0, 0, 0, 0.05);
  cursor: pointer;
  transition: 0.3s;
}
.avatar-item.active {
  box-shadow: 0 10px 15px 0 rgba(0, 0, 0, 0.05);
  transform: translateY(-10px);
}
.avatar-item img {
  width: 100%;
}

input[type="text"] {
  width: 100%;
  border: none;
  padding: 20px 30px;
  box-sizing: border-box;
  outline: none;
  margin-top: 30px;
  font-size: 20px;
  box-shadow: 0 0px 15px 0 rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}
button.submit {
  background: #58a241;
  padding: 20px 80px;
  border: none;
  outline: none;
  color: white;
  font-size: 20px;
  border-radius: 10px;
  width: 100%;
  transition: 0.2s;
  cursor: pointer;
}
button.submit:hover {
  background: #000000;
}

.notice {
  color: #aaa;
  letter-spacing: 1.5px;
}
p {
  font-weight: bold;
  letter-spacing: 1.5px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
