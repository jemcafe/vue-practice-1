<template>
  <div class="hello">
    <div class="basics">
      <!-- {{}} interpolation -->
      {{ name }}

      <!-- v-bind:disabled  (Disables the button) -->
      <button v-bind:disabled="btnState">Change Name</button>

      <ul>
        <!-- looping -->
        <!-- <li v-for="(data, index) in skills" :key="index">{{ index }}. {{ data.skill }}</li> -->
      </ul>

      <!-- if / else -->
      <p v-if="skills.length >= 1">You have more than 1 skill</p>
      <p v-else>You have less than or equal to 1 skill</p>

      <!-- class binding  (The classes are binded and the elements are styled if property values are true)-->
      <div v-bind:class="{ alert: showAlert, 'another-class': showClass }"></div>
      <div v-bind:class="alertObject"></div>

      <!-- style binding -->
      <div v-bind:style="{ backgroundColor: bgColor, width: bgWidth, height: bgHeight }"></div>
    </div>

    <!--  -->
    <div class="holder">

      <form @submit.prevent="addSkill"> <!-- the modifier .prevent works like .prevetDefault() -->
        <!-- The input must be at least 5 characters The input must have a name when using v-validate -->
        <input type="text" placeholder="Enter a skill..." v-model="skill" v-validate="'min:5'" name="skill">

        <!-- A vee-validate error message if the condition is not met. The 'errors' object comes from vee-validate. -->
        <p class="alert" v-if="errors.has('skill')">{{ errors.first('skill') }}</p>

        <!-- transition  (transition is a custom component wrapper. It's great for entering and leaving transitions) -->
        <transition name="alert-in">
          <p class="alert" v-if="errors.has('skill')">{{ errors.first('skill') }}</p>
        </transition>

        <!-- transition animation using an animation library -->
        <transition name="alert-in" enter-active-class="animated flipInX" leave-active-class="animated flipOutX">
          <p class="alert" v-if="errors.has('skill')">{{ errors.first('skill') }}</p>
        </transition>
      </form>

      <ul>
        <!-- <li v-for="(data, index) in skills" :key="index">{{ data.skill }}</li> -->
        <!-- transition animations for elements in a group -->
        <transition-group name="list" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <!-- <li v-for="(data, index) in skills" :key="index">{{ data.skill }}</li> -->
          <li v-for="(data, index) in skills" :key="index">
            {{ data.skill }}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>
        </transition-group>
      </ul>

      <p>The skills you possess</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Skills',  // Component name
  data() {         // State
    return {
      name: 'Bumble',
      btnState: true,
      showAlert: true,
      showClass: true,
      alertObject: {
        alert: true,
        'another-class': true
      },
      bgColor: 'lightblue',
      bgWidth: '100%',
      bgHeight: '30px',

      skills: [
        { "skill": "Vue.js" },
        { "skill": "Front End Developer" },
      ],
      skill: ''
    }
  },
  methods: {  // The methods can't be arrow functions
    addSkill () {
      // This prevents the skill from being added if the input is not valid
      this.$validator.validateAll().then(result => {
        if (result) {
          this.skills.push({ skill: this.skill });
          this.skill = '';
        } else console.log('Not valid');
      })
    },
    remove (id) {
      this.skills.splice(id, 1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>  /* <style src="./Skills.css" scoped>  Importing css */
  .basics {
    background: rgb(234, 255, 234); }
  .alert {
    background-color: lightblue;
    width: 100%;
    padding: 10px;
    box-sizing: border-box; }
  .another-class {
    border: 2px dashed grey; 
    box-sizing: border-box; }

  /* ------------------------ */
  @import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1"; /* animation library */
  @import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"; /* font awesome */

  .holder {
    margin-top: 20px;
    background: #fff;
  }
  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  ul li {
    padding: 20px;
    font-size: 1.3em;
    background-color: #E0EDF4;
    border-left: 5px solid #3EB3F6;
    margin-bottom: 2px;
    color: #3E5252;
  }
  p {
    text-align:center;
    padding: 30px 0;
    color: gray;
  }
  .container {
    box-shadow: 0px 0px 40px lightgray;
  }
  input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #323333;
    color: #687F7F;
  }

  .alert-in-enter-active {
    animation: bounce-in .5s;
  }
  .alert-in-leave-active {
    animation: bounce-in .5s reverse;
  }
  @keyframes bounce-in {
    0% { transform: scale(0); }
    50% { transform: scale(1.5); }
    100% { transform: scale(1); }
  }
  li i {
    float: right;
    cursor: pointer;
  }
</style>
