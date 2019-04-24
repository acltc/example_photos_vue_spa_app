<template>
  <div class="home">
    <h1>New Photo</h1>
    <div>
      Name:
      <input type="text" v-model="newPhotoName" />
      Width:
      <input type="text" v-model="newPhotoWidth" />
      Height:
      <input type="text" v-model="newPhotoHeight" />
      <button v-on:click="createPhoto()">Create Photo</button>
    </div>
    <h1>All Photos</h1>
    <div v-for="photo in photos">
      <h2>{{ photo.name }}</h2>
      <img v-bind:src="photo.url" v-bind:alt="photo.name" />
      <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      photos: [],
      newPhotoName: "",
      newPhotoWidth: "",
      newPhotoHeight: ""
    };
  },
  created: function() {
    axios.get("/api/photos").then(response => {
      this.photos = response.data;
    });
  },
  methods: {
    createPhoto: function() {
      var params = {
        name: this.newPhotoName,
        width: this.newPhotoWidth,
        height: this.newPhotoHeight
      };
      axios.post("/api/photos", params).then(response => {
        this.photos.push(response.data);
        this.newPhotoName = "";
        this.newPhotoWidth = "";
        this.newPhotoHeight = "";
      });
    }
  }
};
</script>
