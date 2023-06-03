<template>
  <div class="text-center">
    <v-row justify="center">
    <v-dialog 
      v-model="localIsDelete"
      persistent
    >
      <v-card
        class="white"
      >
        <v-card-text class="text-h5  white">
          <div class="pt-4"><label style="white-space: pre-line" id="text" class="black--text"> Select the group </label></div>
        </v-card-text>
        <v-card-text>
          <v-row>
            <v-col cols="1"></v-col>
            <v-col cols="10">
              <v-select
                label="Group"
                v-model="selected"
                :items="groupList"
                item-text="name"
                @change="checkError"
              ></v-select>
            </v-col>
            <v-col cols="1"></v-col>
          </v-row>
          <v-row>
            <v-col cols="1"></v-col>
            <span v-if="showErr" class="error--text">Select a Group</span>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer> 
          <v-btn
            color="red"
            text
            @click="deleteItem"
          >
            Delete
          </v-btn>
          <v-btn
            color="blue"
            text
            @click="closeMSG"
          >
            close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
  </div>
</template>
<script>
export default {
    props:{ isDelete: Boolean, groupList: Array },
    data: () => ({
      selected: '',
      showErr: false
    }),
    methods:{
      closeMSG(){
          this.$emit('closeDelete')
      },
      deleteItem(){
        if(!this.selected){
          this.showErr = true
        } else {
          this.showErr = false
          this.$emit('deleteGroup', this.selected)
          this.selected = ''
        }
      },
      checkError(){
        if(this.selected){
          this.showErr = false
        }
      }
    },
    computed:{
      localIsDelete:{
        get(){
          return this.isDelete
        }
      },
      localDeleteText:{
        get(){
          return this.DeleteText
        }
      },
      localstatus:{
        get(){
          return this.status
        }
      }
    }
}
</script>
<style scoped>
#text {
  font-size: 20px !important;
}
</style>