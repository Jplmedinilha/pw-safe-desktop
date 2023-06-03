<template>
    <v-container>
      <ResultMsg
        :isSnackbar="snackBar.isSnackbar"
        :snackbarText="snackBar.snackbarText"
        :status="snackBar.snackStatus"
        v-on:closeMsgResult="closeMsgResult"
      />
      <SysLoader 
        :isLoader="isLoader" 
      />
      <v-row v-if="this.onActiveTab == 0">
        <v-col cols="1">
        </v-col>
        <v-col cols="10">
          <h3 color="red" >Welcome {{userName}} </h3>
        </v-col>
        <v-col cols="1">
        </v-col>
      </v-row>

      <v-row fluid>
        <v-col cols="1">
        </v-col>
        <v-col cols="10" no-gutters class="pa-0 mt-2 mb-1">
            <v-tabs v-model="active_tab" left dark height="40px" background-color="#fff" @change="tabChanged">
            <v-tabs-slider color="secondary"></v-tabs-slider>
            <v-tab class="rounded elevation-5" :class="this.onActiveTab == 0 ? 'orange' : 'secondary'">Credentials</v-tab>
            <v-tab class="rounded elevation-5" :class="this.onActiveTab == 1 ? 'orange ml-1 elevation-3' : 'secondary ml-1'">+</v-tab>       
            <v-tab-item>    
                <v-container fluid class="pa-0">
                <v-row class="table-container" v-show="true">
                    <v-col cols="12" sm="12" class="mt-2 mb-2 elevation-5">
                    <v-data-table
                        dense
                        :headers="headers"
                        :items="items"
                        height="290"
                        class="elevation-5 rounded"
                        :hide-default-footer="true"
                    >
                    <template v-slot:[`item.groupName`]="{ item }">
                      <span :class="(item.critical_flg == 'Y') ? 'red--text' : 'grey--text'">{{ item.groupName }}</span>
                    </template>
                    <template v-slot:[`item.systemName`]="{ item }">
                      <span class="pointer primary--text" @click="openModal(item)">{{ item.systemName }}</span>
                    </template>
                    </v-data-table>
                    </v-col>
                </v-row>
                </v-container>
            </v-tab-item>
            <v-tab-item>
              <v-radio-group @change="getGroups" v-model="inline" hide-details>
                <v-radio label="New Credendial" value="1"></v-radio>
                <v-radio label="New Group" value="2"></v-radio>
              </v-radio-group>
              <template v-if="inline == 1">
                <!-- NEW CREDENTIAL -->
                <v-sheet class="mx-auto">
                  <v-form>
                    <v-select
                      label="Group"
                      :items="groups"
                      item-text="name"
                      v-model="selGroup"
                    ></v-select>
                    <v-text-field
                      v-model="newSystem"
                      label="System Name"
                    ></v-text-field>
                    <v-text-field
                      v-model="newUser"
                      label="UserName"
                    ></v-text-field>
                    <v-text-field
                      v-model="newPassword"
                      label="New Password"
                    ></v-text-field>
                  </v-form>
                </v-sheet>
              </template>
              <template v-if="inline == 2">
                <!-- NEW GROUP -->
                <v-sheet class="mx-auto mt-10">
                  <v-form>
                    <v-text-field
                      v-model="groupName"
                      label="Group name"
                    ></v-text-field>
                    <v-checkbox v-model="criticalFlag" label="Critical"></v-checkbox>
                    <v-row>
                      <v-col cols="2"></v-col>
                      <v-col cols="8">
                          <v-btn @click="openDeleteGroup" block small color="grey">Delete a Group</v-btn>
                      </v-col>
                      <v-col cols="2"></v-col>
                    </v-row>
                  </v-form>
                </v-sheet>
              </template>
            </v-tab-item>
            </v-tabs>
        </v-col>
        </v-row>
        <v-row fluid>
          <v-col cols="1"></v-col>
          <v-col :cols="this.onActiveTab == 1 ? 5 : 10">
              <v-btn @click="logOut" block  color="red">LogOut</v-btn>
          </v-col>
          <v-col v-if="this.onActiveTab == 1 && inline != 0" cols="5">
              <v-btn @click="add" block  color="green">Add</v-btn>
          </v-col>
          <v-col cols="1"></v-col>
            
        </v-row>
        <DeleteForm
          :isDelete="Delete.isDelete"
          :groupList="groups"
          v-on:closeDelete="closeDelete"
          v-on:deleteGroup="deleteGroup"
        />
        <ConfirmDeleteMsg
          :isSnackbar="ConfirmMsg.isConf"
          :confirmText="ConfirmMsg.confirmText"
          :confirmStatus="ConfirmMsg.confirmStatus"
          v-on:onDeleteCanceled="onConfirmCancel"
          v-on:onDeleteConfirmed="onDeleteGroupConfirm"
        />
      
    </v-container>
  </template>
  <script>
import ResultMsg from "../components/ResultMsg.vue";
import SysLoader from "../components/SysLoader.vue";
import DeleteForm from "../components/DeleteForm.vue";
import ConfirmDeleteMsg from "../components/ConfirmDeleteMsg.vue";
  
    export default {
      name: 'Main_form',
      props:{ userName: String },
      components: {
        SysLoader,
        ResultMsg,
        DeleteForm,
        ConfirmDeleteMsg
      },
      data: () => ({
        snackBar: {
          isSnackbar: false,
          snackStatus: "",
          snackbarText: "",
        },
        ConfirmMsg: {
          isConf: false,
          confirmText: "",
          confirmStatus: ''
        },
        Delete: {
          isDelete: false,
        },
        isLoader: false,
        pwShow: false,
        password: '',
        activeTab: 0,
        active_tab: 0,
        headers: [
            { value: 'groupName', text: 'Group', sortable: true},
            { value: 'systemName', text: 'System', sortable: false }
        ],
        items: [],
        inline: 0,
        groupName: '',
        criticalFlag: false,
        groups: [],
        selGroup: '',
        newSystem: '',
        newUser: '',
        newPassword: '',
        itemToBeDeleted: ''
      }),
      mounted(){
        this.getCredentials();
      },
      methods: {
        tabChanged(data) {
            this.activeTab = data;
            if(data == 0) { //mudou pra credentials tab
              this.getCredentials()
            }
        },
        logOut(){
          this.$emit('logStsChange', this.userName)
        },
        closeMsgResult(){
          this.snackBar.snackStatus = ""
          this.snackBar.snackbarText = ""
          this.snackBar.isSnackbar = false
        },
        closeDelete(){
          this.Delete.DeleteStatus = ""
          this.Delete.DeleteText = ""
          this.Delete.isDelete = false
        },
        clearFields(){
          this.groupName = ''
          this.criticalFlag = false

          this.selGroup = ''
          this.newSystem = ''
          this.newUser = ''
          this.newPassword = ''

        },
        async add(){
          if(this.inline == 1){

            if(!this.selGroup || !this.newSystem || !this.newUser || !this.newPassword){
              this.snackBar.snackStatus = "E"
              this.snackBar.snackbarText = "Missing Information"
              this.snackBar.isSnackbar = true
            } else {

              this.isLoader = true

              const params = {
                userName: this.userName,
                groupName: this.selGroup,
                systemName: this.newSystem,
                cred_user: this.newUser,
                cred_pw: this.newPassword,
              }
              
              await this.axios.post(
                `http://localhost:3000/api/addCredentials`, params
              )
              .then((res) =>{
                this.snackBar.snackStatus = ""
                this.snackBar.snackbarText = res.data.RETURN.affectedRows + " New credential created"
                this.snackBar.isSnackbar = true
                this.isLoader = false

                this.clearFields();
              })
              .catch((err)=>{
                console.log(err)
                this.isLoader = false
              })
            }
            
          } else if(this.inline == 2){
            
            if(!this.groupName){
              this.snackBar.snackStatus = "E"
              this.snackBar.snackbarText = "Insert Group Name"
              this.snackBar.isSnackbar = true
            } else {

              this.isLoader = true

              const params = {
                group_name: this.groupName,
                critial_flg: (this.criticalFlag) ? 'Y' : 'N'
              }
              await this.axios.post(
                `http://localhost:3000/api/addGroup`, params
              )
              .then((res) =>{
                this.snackBar.snackStatus = ""
                this.snackBar.snackbarText = "Inserted Groups: " + res.data.RETURN.affectedRows
                this.snackBar.isSnackbar = true
                this.isLoader = false

                this.clearFields();
              })
              .catch((err)=>{
                console.log(err)
                this.isLoader = false
              })
            }

          }
        },
        async getGroups(){
          if(this.inline == 1){
            this.isLoader = true
            await this.axios.get(
              `http://localhost:3000/api/groupList`
            )
            .then((res) =>{
              this.groups = res.data.RETURN
              this.isLoader = false
            })
            .catch((err)=>{
              console.log(err)
              this.isLoader = false
            })
          }
        },
        async getGroupForce(){
          this.isLoader = true
          await this.axios.get(
            `http://localhost:3000/api/groupList`
          )
          .then((res) =>{
            this.groups = res.data.RETURN
            this.isLoader = false
          })
          .catch((err)=>{
            console.log(err)
            this.isLoader = false
          })
        },
        openDeleteGroup(){
          this.getGroupForce();
          this.Delete.isDelete = true
        },
        deleteGroup(item){
          this.ConfirmMsg.confirmText = "Are you sure you want to delete '" + item + "'?"
          this.ConfirmMsg.confirmStatus = "E"
          this.itemToBeDeleted = item
          this.ConfirmMsg.isConf = true
        },
        onConfirmCancel(){
          this.ConfirmMsg.confirmText = ""
          this.ConfirmMsg.confirmStatus = ""
          this.ConfirmMsg.isConf = false

        },
        async onDeleteGroupConfirm(){
          this.isLoader = true
            await this.axios.delete(
              `http://localhost:3000/api/deleteGroup/${this.itemToBeDeleted}`
            )
            .then((res) =>{
              this.snackBar.snackStatus = ""
              this.snackBar.snackbarText = "Deleted Groups: " + res.data.RETURN.affectedRows
              this.snackBar.isSnackbar = true
              this.ConfirmMsg.isConf = false
              this.ConfirmMsg.isConf = false

              this.isLoader = false
              this.clearFields();
              
            })
            .catch((err)=>{
              console.log(err)
              this.isLoader = false
            })
        },
        async getCredentials(){
          this.isLoader = true
          await this.axios.get(
            `http://localhost:3000/api/getCredentials/${this.userName}`
          )
          .then((res) =>{
            this.items = res.data.RETURN
            this.isLoader = false
          })
          .catch((err)=>{
            console.log(err)
            this.isLoader = false
          })
        },
        openModal(){
          console.log('oi')
        }
        
      },
      computed: {
        onActiveTab() {
            return this.activeTab;
        },
      },
    }
  </script>
<style>
.pointer{
  cursor: pointer;
}
</style>