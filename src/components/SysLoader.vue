<template>
  <div class="text-center">
    <v-dialog
      v-model="localIsLoader"
      hide-overlay
      persistent
     
      width="500"
    >
      <v-card
        color="primary"
        dark
      > 
        <v-card-text class="pt-5">
          Please stand by
          <v-progress-linear
            indeterminate bottom active
            color="white"
            class="mb-0"
          ></v-progress-linear>
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-overlay :value="localIsLoader"></v-overlay>
  </div>
</template>
<script>
  export default {
    props:{ isLoader: Boolean},
    data () {
      return {
        dialog: true,
      }
    },
    methods:{
      close(){
        this.$emit("closeLoader")
      },
    },
    computed:{
      localIsLoader:{
        get(){
          return this.isLoader
        }
      }
    }
  }
</script>