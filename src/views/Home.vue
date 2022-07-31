/* eslint-disable prettier/prettier */

<template>
  <div class="container mt-5">
    <h2 v-if="!user">Welcome, please log in or register</h2>
    <div v-if="user" style="height: 600px">
      <div class="heading text-center h2">
        Todo List
      </div>
      <div class="row">
        <div class="col-md-9">
          <input type="text" class="form-control" v-model="list.name">
        </div>
        <div class="col-md-3">
          <button class="btn btn-success btn-block" @click="addList()">Add</button>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <lists 
            :lists="lists"
            v-on:reloadlist="getList()"
          />
        </div>
      </div>
    </div>    
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import axios from "axios";
import lists from './lists';
export default {
  name: "Home",
  components : {
    lists
  },

  data() {
    return {
      success: null,
      error: null,
      list : {
        name : ""
      },
      lists : []
    };
  },

  computed: {
    ...mapGetters("auth", ["user"])
  },

  methods: {
    ...mapActions("auth", ["sendVerifyResendRequest"]),

    verifyResend() {
      this.success = this.error = null;
      this.sendVerifyResendRequest()
        .then(() => {
          this.success =
            "A fresh verification link has been sent to your email address.";
        })
        .catch(error => {
          this.error = "Error sending verification link.";
          console.log(error.response);
        });
    },
    addList() {
      if (this.list.name === '')
        return

      axios.post( process.env.VUE_APP_API_URL + 'list/store', {
        list : this.list
      })
      .then( res => {
        if (res.status === 201) {
          this.list.name = ""
          this.getList()
        }
      })
    },
    getList() {
      axios.get( process.env.VUE_APP_API_URL + 'lists')
      .then( res => {
        this.lists =  res.data
      })
    }
  },
  created() {
    this.getList()
  }
};
</script>

<style scoped>
  .heading {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #FF5631;
    color: #FFF;
    height: 80px;
  }
</style>
