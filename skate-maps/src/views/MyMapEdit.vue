<template>
  <div class="mymapedit">
    <div class="mymapedit-card">
      <h1 class="mymapedit-title">プロフィール編集</h1>
      <div v-if="confirmedImage">
        <button @click="deletePreview()">X</button>
        <img :src="confirmedImage" class="profile-img">
      </div>
      <div class="form">
        <input placeholder="ユーザーネーム" type="text" v-model="name" />
        <input placeholder="プロフィール" type="text" v-model="profile" />
        <input placeholder="住所" type="text" v-model="address" />
        <p><input type="file" @change="confirmImage"></p>
        <button @click="userEdit()">編集</button>
      </div>
      <img src="" id="img">
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      name: '',
      profile: '',
      address: '',
      fileInfo: '',
      confirmedImage: ''
    }
  },
  methods: {
    confirmImage(e) {
      console.log(e);
      this.fileInfo = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(this.fileInfo);

      reader.onload = e => {
        this.confirmedImage = e.target.result;
        console.log(this.confirmedImage);
      }
    },
    deletePreview() {
      this.confirmedImage = '';
    },
    userEdit() {
      const formData = new FormData();
      formData.append('email', '@a');
      formData.append('name', this.name);
      formData.append('profile', this.profile);
      formData.append('address', this.address);
      formData.append('file', this.confirmedImage);

      // console.log(formData.get());
      for (let value of formData.entries()) { 
        console.log(value); 
      }
      console.log(this.fileInfo);

      axios.put("http://127.0.0.1:8000/api/user",{
        name: this.name,
        profile: this.profile,
        address: this.address,
        file: this.confirmedImage
      })
      .then((response) => {
        console.log(response);
        console.log(response.data.data.image);
        const decodeData = response.data.data.image;
        this.$store.dispatch("updateUserData");
        const img = document.getElementById("img");
        img.setAttribute('src', decodeData);
      })
    }
  }
}
</script>

<style scoped>
.mymapedit {
  height: 700px;
}
.mymapedit-card {
  height: 500px;
  width: 70%;
  margin: 100px auto;
  border: 1px solid #000;
  border-radius: 5px;
}
.mymapedit-title {
  margin-top: 10px;
  text-align: center;
}
.profile-img {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  margin: 100px 0 50px;
}
</style>
