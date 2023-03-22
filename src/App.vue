<template>
  <div class="columns mt-5">
    <div class="column is-1"></div>
    <div class="column">
      <div class="control">
        <input class="is-range" type="range" min="8" max="16" v-model="rangePassword">
      </div>
      <div class="control">
        <label>Range: {{ rangePassword }}</label>
      </div>
      <div class="control mt-3">
        <label class="checkbox">
          <input type="checkbox" v-model="checkCapitals">
          Include Capital Letters
        </label>
      </div>
      <div class="control mt-3">
        <label class="checkbox">
          <input type="checkbox" v-model="checkNumbers">
          Include Numbers
        </label>
      </div>
      <div class="control mt-3">
        <label class="checkbox">
          <input type="checkbox" v-model="checkSymbols">
          Include Symbols
        </label>
      </div>
      <div class="control mt-3">
        <button class="button is-info" @click="generatePassword()">Generate Password</button>
      </div>
    </div>
    <div class="column">
      <h4><b>Your New Password:</b></h4>
      <div class="control my-copy">
        <input id="input-password" class="input" type="text" disabled v-model="newPassword">
        <button class="button is-info is-right" @click="copyPassword()" v-bind:disabled="!newPassword">Copy</button>
      </div>
    </div>
    <div class="column is-1"></div>
  </div>
</template>

<script>
import Swal from 'sweetalert2';

export default {
  data() {
    return {
      rangePassword: 8,
      checkCapitals: false,
      checkNumbers: false,
      checkSymbols: false,
      newPassword: "",
    }
  },
  methods: {
    checkCharaters() {
      let elements = "abcdefghijklmnopqrstuvwxyz";
      const capitals = elements.toUpperCase();
      const numbers = "0123456789";
      const symbols = `|°¬#$%&/=¿?¡!*+^-_`;

      if (this.checkCapitals) {
        elements += capitals;
      }

      if (this.checkNumbers) {
        elements += numbers;
      }

      if (this.checkSymbols) {
        elements += symbols;
      }

      return elements;
    },
    generatePassword() {
      const characters = this.checkCharaters();
      let password = "";
      
      for (let i = 0; i < this.rangePassword; i++) {
        password += characters.charAt(Math.floor(Math.random() * characters.length));
      }

      this.newPassword = password;
    },
    async copyPassword() {
      const Toast = Swal.mixin({
        toast: true,
        position: 'bottom-end',
        showConfirmButton: false,
        timer: 2000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener('mouseenter', Swal.stopTimer)
          toast.addEventListener('mouseleave', Swal.resumeTimer)
        }
      })

      await navigator.clipboard.writeText(this.newPassword);
      Toast.fire({
        icon: 'success',
        title: 'Copied!'
      })
    }
  }
}
</script>

<style>
.is-range{
  width: 80%;
}

.my-copy input{
  width: 80%;
  cursor: text!important;
}

.my-copy button{
  width: 20%;
}
</style>