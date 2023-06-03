<template>
    <v-container>
      <v-row>
        <v-col cols="1">
        </v-col>
        <v-col cols="10">
          <h3 color="red">Welcome {{userName}} </h3>
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
            <!-- <v-tab class="rounded elevation-5" :class="this.onActiveTab == 0 ? 'orange' : 'secondary'">PW List</v-tab> -->
            <v-tab class="rounded elevation-5" :class="this.onActiveTab == 0 ? 'orange' : 'secondary'">Passwords</v-tab>
            <v-tab class="rounded elevation-5" :class="this.onActiveTab == 1 ? 'orange ml-1 elevation-3' : 'secondary ml-1'">ADD</v-tab>         
            <v-tab-item>    
                <v-container fluid class="pa-0">
                <v-row class="table-container" v-show="true">
                    <v-col cols="12" sm="12" class="mt-2 mb-2 elevation-5">
                    <v-data-table
                        dense
                        :headers="headers"
                        :items="items"
                        height="300"
                        class="elevation-5 rounded"
                        :hide-default-footer="true"
                    >
                    </v-data-table>
                    <!-- <template>
                        <v-table
                            fixed-header
                            dense
                            height="300px"
                        >
                            <thead>
                                <tr>
                                    <th class="">
                                        Group
                                    </th>
                                    <th class="">
                                        System
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="item in items"
                                    :key="item.name"
                                >
                                    <td>{{ item.GROUP }}</td>
                                    <td>{{ item.SYSTEM }}</td>
                                </tr>
                            </tbody>
                        </v-table>
                    </template> -->
                    </v-col>
                </v-row>
                </v-container>
            </v-tab-item>
            <v-tab-item>
                <v-container fluid class="pa-0">
                <v-row class="table-container" v-show="true">
                    <v-col cols="12" sm="12" class="mt-2 mb-2 elevation-5">
                    
                    </v-col>
                </v-row>
                </v-container>
            </v-tab-item>
            <v-tab-item>
                <v-container fluid class="pa-0">
                <v-row class="table-container" v-show="true">
                    <v-col cols="12" sm="12" class="mt-2 mb-2 elevation-5">
                    
                    </v-col>
                </v-row>
                </v-container>
            </v-tab-item>
            </v-tabs>
        </v-col>
        </v-row>
    
      
    </v-container>
  </template>
  <script>
    export default {
      name: 'Main_form',
      props:{ userName: String },
      data: () => ({
        pwShow: false,
        password: '',
        userNm: '',
        activeTab: 0,
        active_tab: 0,
        headers: [
            { value: 'GROUP', text: 'Group', sortable: false },
            { value: 'SYSTEM', text: 'System', sortable: false }
        ],
        items: [{
            GROUP: 'OI',
            SYSTEM: 'OI'
        }]
      }),
      methods: {
        async login(){
          this.axios.get(
            `http://localhost:3000/api/login?username=${this.userNm}&pw=${this.password}`
          )
          .then((res) =>{
            console.log(res.data.RETURN.CNT)
          })
          .catch((err)=>{
            console.log(err)
          })
        },
        tabChanged(data) {
            this.activeTab = data;
        },
      },
      computed: {
        onActiveTab() {
            return this.activeTab;
        },
      },
    }
  </script>
<style>

</style>