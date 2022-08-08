<template>
  <div id="app">
    <form id="myForm">
      <h2>Access Card Authorization Form</h2>

      <p class="errorMessage" v-if="missingFields">
        Please fill out all form fields
      </p>

      <input
        type="text"
        id="name"
        name="name"
        v-model="formObject.name"
        placeholder="FULL NAME"
      />
      <br /><br />

      <input
        type="text"
        id="position"
        name="position"
        v-model="formObject.position"
        placeholder="POSITION"
      />
      <br /><br />

      <input
        type="text"
        id="company"
        name="company"
        v-model="formObject.company"
        placeholder="COMPANY"
      />
      <br /><br />
    </form>

    <button id="submitButton" @click="checkFields()">SUBMIT</button>

    <div id="listContainer">
      <ul>
        <li v-for="(user, index) in usersList" :key="index">
          {{ user.name }}

          <button id="cardButton" @click="showDetails(index)">OPEN</button>
          <!-- create button for each, guess same index as v-for..., so each bullet's button hv diff index as argument -->
        </li>
      </ul>
    </div>

    <div v-if="isShowDetails" id="myModal" class="modal">
      <Tilt
        :options="{
          perspective: 1000,
          transition: true,
          max: 25,
          glare: true,
          'max-glare': 1,
        }"
      >
        <div class="modal-content">
          <span class="close" @click="closeModal">&times;</span>
          <h1 id="cardLabel">STARK INDUSTRIES</h1>
          <p id="cardTitle">SECURITY PASS</p>
          <div id="text">
            <p>
              <span id="modalText1">{{ modalContent.name }}</span>
            </p>
            <p>
              <span id="modalText2">{{ modalContent.position }}</span>
            </p>
            <p>
              <span id="modalText3">{{ modalContent.company }}</span>
            </p>
            <p>
              ID: <span> {{ modalContent.randomID }}</span>
            </p>
            <p>
              ACCESS LEVEL: <span>{{ modalContent.randomAccess }} </span>
            </p>
            <img src="../assets/globe.png" />
            <p id="barcode">DEFAULTT</p>
          </div>
        </div>
      </Tilt>
    </div>
  </div>
</template>

<script>
import Tilt from "vanilla-tilt-vue";

export default {
  name: "App",
  data() {
    return {
      formObject: {
        name: null,
        position: null,
        company: null,
        randomID: null,
        randomAcces: null,
      },
      usersList: [],
      isShowDetails: false,
      modalContent: null,
      missingFields: false,
    };
  },
  components: { Tilt },
  props: {
    options: Object,
  },
  methods: {
    checkFields() {
      if (
        this.formObject.name === "" ||
        this.formObject.name === null ||
        this.formObject.position === "" ||
        this.formObject.position === null ||
        this.formObject.company === "" ||
        this.formObject.company === null
      ) {
        this.missingFields = true;
        return false;
      } else {
        this.missingFields = false;
        this.createList();
      }
    },
    createList() {
      // const obj = this.formObj doesnt work
      const obj = JSON.parse(JSON.stringify(this.formObject)); // The JSON. parse() function is used to convert a string into a JavaScript object
      // console.log("obj: " + obj);
      obj.randomID = this.generateID();
      obj.randomAccess = this.generateAccess();
      this.usersList.push(obj); //push with randomID and randomACcess
      this.formObject = {
        name: "",
        position: "",
        company: "",
        // randomID: "",
        // randomAccess: "",
      };
    },
    showDetails(id) {
      this.isShowDetails = true;
      this.modalContent = this.usersList.find((obj, index) => index === id); // show obj with index that equal to id
    },
    closeModal() {
      this.isShowDetails = false;
    },
    generateID() {
      this.randomID = Math.floor(Math.random() * 1000000 + 1); // 6 digits
      return this.randomID;
    },
    generateAccess() {
      this.randomAccess = Math.floor(Math.random() * (5 - 1) + 1); // 1 to 5
      return this.randomAccess;
    },
  },
};
</script>

<style>
body {
  text-align: center;
  background-color: black;
}

form {
  color: white;
  font-family: "Orbitron", sans-serif !important;
}

input[type="text"] {
  height: 20px;
  width: 250px;
  font-size: 12px;
  text-indent: 10px;
  font-family: "Orbitron", sans-serif !important;
  color: white;
  box-shadow: 0 0 15px #9ecaed;
  background-color: black;
  border-radius: 6px;
  border: 1px solid green; /* 0.01px solid white; */
}

input[type="text"]:focus {
  box-shadow: 0 0 10px red;
  border: 0.01px solid black;
  outline: 0.01px solid black;
}

::placeholder {
  color: white;
}

.errorMessage {
  font-size: 14px;
  color: red;
}

#submitButton,
#cardButton {
  width: 80px;
  height: 30px;
  font-size: 12px;
  font-family: "Orbitron", sans-serif !important;
  color: white;
  box-shadow: 0 0 15px #9ecaed;
  background-color: black;
  border-radius: 6px;
  border: 1px solid green;
}

#cardButton {
  margin-top: 5px;
  margin-left: 10px;
}

#submitButton:active,
#cardButton:active {
  color: black;
  background-color: limegreen;
}

#listContainer {
  text-align: center;
}

ul {
  display: inline-block;
  text-align: left;
  font-size: 12px;
  font-family: "Orbitron", sans-serif !important;
  color: white;
}

/* The Modal (background) */
.modal {
  display: inline-block; /* Hidden by default */
  position: fixed; /* Stay in place */
  width: 300px; /* need? QQ 100%; // Full width */
  height: 450px; /* need? QQ 100%; // Full height */
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1; /* Sit on top */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
  /* left: 0;
  top: 0; 
  padding-top: 100px; // Location of the box */
}

/* Modal Content */
.modal-content {
  width: 300px;
  height: 450px;
  margin: auto;
  padding: 20px;
  text-align: center;
  background-image: linear-gradient(315deg, #2d3436 0%, #d3d3d3 74%);
  border-radius: 10px;
  border: 1px solid white;
}

/* The Close Button */
.close {
  float: right;
  font-size: 28px;
  font-weight: bold;
  color: rgb(58, 57, 57);
}

.close:hover,
.close:focus {
  cursor: pointer;
  text-decoration: none;
  text-shadow: 0 0 10px #fff, 0 0 10px #fff, 0 0 21px #fff;
  color: #000;
}

#cardLabel {
  position: relative;
  top: 10%;
  transform: rotate(-180deg);
  writing-mode: tb-rl;
  padding: 0;
  margin: 0;
  line-height: 0;
  font-size: 30px;
  font-family: "Faster One", cursive;
}

#cardTitle {
  margin-top: -115%;
  /* transform: translateZ(20px); hard to click */
  font-size: 35px;
  font-family: impact;
  text-decoration: underline;
}

#text {
  margin-top: -3%;
  line-height: 23px;
  font-size: 23px;
  font-family: impact;
}

img {
  width: 100px;
  height: 100px;
  transform: translate(-5%, 5%);
}

#barcode {
  bottom: 0%;
  transform: translateY(160%);
  margin-top: 0;
  padding-top: 0;
  font-size: 60px;
  font-family: "Libre Barcode 39", cursive;
}
</style>

<!--
-card how get rid of bar on right 
-not sure if correct way form validation
-best practice order of properties in vue
-->