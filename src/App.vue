<template>
  <div class="todos__wrapper">
    <h1 class="heading">
      Populer Heros <span v-text="`${heroList.length > 0 ? getHerosLength : ''}`"></span>
    </h1>
    <div class="todos__body">
      <p class="message">{{ message.error }}</p>
      <p class="message">{{ message.success }}</p>
      <ul>
        <li v-for="hero in heroList" :key="hero.id">
          {{ hero.name }} <span @click="removeItem(hero.id)">x</span>
        </li>
      </ul>
    </div>
    <div class="todos__footer">
      <form @submit.prevent="addHero">
        <div class="form-gorup">
          <input type="text" v-model="inputData" />
          <button type="submit">Add Hero</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputData: "",
      heroList: JSON.parse(localStorage.getItem("heros")) || [],
      message: {
        error: "",
        success: "",
      },
    };
  },
  computed: {
    getHerosLength() {
      return this.heroList.length;
    },
  },
  methods: {
    addHero() {
      if (this.inputData == "") {
        this.message.error = "Please enter value first 🙄";
        this.message.success = "";
        setTimeout(() => {
          this.message.success = "";
          this.message.error = "";
        }, 1000);
      } else {
        this.message.success = "Item Added successfully 😍";
        this.message.error = "";
        setTimeout(() => {
          this.message.success = "";
          this.message.error = "";
        }, 1000);
        const newHero = {
          id: 1,
          name: this.inputData,
        };
        let id = this.heroList[this.heroList.length - 1]?.id + 1 || 1;
        newHero.id = id;
        this.heroList.push(newHero);
        this.inputData = "";
        localStorage.setItem("heros", JSON.stringify(this.heroList));
      }
    },
    removeItem(id) {
      this.heroList = this.heroList.filter((hero) => hero.id !== id);
      localStorage.setItem("heros", JSON.stringify(this.heroList));
    },
  },
};
</script>

<style>
ul>li span {
  cursor: pointer;
  background: red;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 5px;
  color: #fff;
}
.todos__wrapper {
  background: rgba(0, 255, 255, 0.274);
  width: 500px;
  height: auto;
  margin: 100px auto;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0px 0px 5px 5px whitesmoke;
}
.heading {
  text-align: center;
}
ul > li {
  list-style-type: none;
  font-size: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-weight: bolder;
  margin-bottom: 5px;
}
.todos__body {
  height: 300px;
  overflow-y: auto;
}
.form-gorup {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
input {
  padding: 10px;
  border: 1px solid rgb(4, 155, 155);
  flex: 2;
  margin-right: 10px;
}
button {
  padding: 10px;
  border: 1px solid rgb(4, 155, 155);
  cursor: pointer;
  flex: 1;
}
.message {
  margin-bottom: 5px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 20px;
  font-weight: bolder;
  text-align: center;
}
</style>
