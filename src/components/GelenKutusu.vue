<template>
  <v-app>
    
  <v-row>
  <v-col
  sm="4"
  md="2"
  lg="2">
 <v-card
   width="25vh"
   height="100%">
   <v-list v-model:opened="open">
     <v-img
     src="https://pbs.twimg.com/profile_images/649530762971115520/j2f8FcG3_400x400.png">
     </v-img>
     <v-list-item prepend-icon="mdi-home" title="Anasayfa"></v-list-item>
     <v-list-item prepend-icon="mdi-receipt" title="Fatura İşlemleri"></v-list-item>
     <v-list-group value="e-Fatura">
       <template v-slot:activator="{ props }">
         <v-list-item
           v-bind="props"
           prepend-icon="mdi-note"
           title="e-Fatura">
       </v-list-item>
       </template>
       <v-list-item to="GelenKutusu" prepend-icon="mdi-email" title="">Gelen Kutusu</v-list-item>
       <v-list-item to="GidenKutusu" prepend-icon="mdi-email" title="">Giden Kutusu</v-list-item>
       <v-list-item prepend-icon="mdi-archive" title="Arşiv"></v-list-item>  
       <v-list-item prepend-icon="mdi-inbox-multiple" title="Eski Faturalar"></v-list-item>
       <v-list-item prepend-icon="mdi-card-account-details" title="Tanımlamalar"></v-list-item>
     </v-list-group>
     <v-list-item prepend-icon="mdi-archive" title="e-Arşiv"></v-list-item>
     <v-list-item prepend-icon="mdi-list-status" title="e-İrsaliye"></v-list-item>
     <v-list-item prepend-icon="mdi-notebook" title="e-Defter"></v-list-item>
     <v-list-item prepend-icon="mdi-script" title="e-SMM"></v-list-item>
     <v-list-item prepend-icon="mdi-script" title="e-MM"></v-list-item>
     <v-list-group value="Tanımlamalar">
       <template v-slot:activator="{ props }">
         <v-list-item
           v-bind="props"
           prepend-icon="mdi-card-account-details"
           title="Tanımlamar">
       </v-list-item>
       </template>
       <v-list-item to="StocksPage" prepend-icon="mdi-store" title="Stoklar"></v-list-item>
       <v-list-item to="CustomersPage" prepend-icon="mdi-account-multiple" title="Müşteriler"></v-list-item>
     </v-list-group>
     <v-list-item prepend-icon="mdi-chart-box" title="Raporlar"></v-list-item>
   </v-list>
 </v-card>
</v-col>

<v-col

 sm="8"
  md="10"
  lg="10">
  
  <div id="app">
      <v-app>
          <v-toolbar
          dense
          floating 
        ><v-text-field
            prepend-icon="mdi-magnify"
            class="mt-4 ml-2"
            single-line
          ></v-text-field>
        </v-toolbar>
  <v-table>
    <thead>
      <tr>
        <th class="text-left">
          <input type="checkbox" name="checkbox"/>
        </th>
        <th class="text-left" style="width:100px">
         
        </th>
        <th class="text-left">
          FATURA BİLGİSİ
        </th>
        <th class="text-left">
         TARİH
        </th>
        <th class="text-left">
         GÖNDERİCİ BİLGİSİ       </th>
        <th class="text-left">
         FATURA DURUMU         </th>
        <th class="text-left">
          CEVAP
        </th>
        
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in items.Content"
        :key="item.InvoiceNumber">
        <td>
      <v-row justify="space-around">
        <input type="checkbox" name="checkbox"/>
      </v-row>
     
        </td> 
               <td>
      <v-row justify="space-around">
          <v-icon
      large
      color="green darken-2"
    >
      mdi-script
    </v-icon>
    <v-icon
      large
      color="green darken-2"
    >
    mdi-magnify
    </v-icon>
      </v-row>
     
        </td>

        <td>FTT {{ item.InvoiceNumber}}<br> <!--Fatura Bilgisi-->
          <v-btn
          size="small"
        rounded="pill"
        color="success"
        >
      {{item.InvoiceType}}
      </v-btn>  / {{item.PayableAmount}} TRY
        
          
        </td>
        <td> <strong> Fatura: </strong>{{ item.IssueDate }} <br> <!--Fatura ve Zarf-->
          <strong> Zarf: </strong> {{ item.EnvelopeUUID}}</td>
          <td>{{ item.SenderName }} <!--Gönderici bilgisi--> <br>
       <strong> Vergi No: </strong>{{item.EnvelopeDate}} <!--Vergin No--> </td>
       <td>{{ item.StatusDetail }}  <!--Alındı Yanıtı Gönderildi--></td>
        {{ item.Phone }}  <!--KDV-->
        <td><b v-if="item.AnswerCode=='waitingForApproval'">Bekliyor</b>
          <b v-else-if="item.AnswerCode=='approved'"> Onaylandı</b>
          <b v-else-if="item.AnswerCode=='rejected'">Kabul Edilmedi</b>
          <b v-else-if="item.AnswerCode=='unknown'">Bilinmiyor</b>
          </td> 
      </tr>
    </tbody>
  </v-table>
</v-app>
</div>
</v-col>

</v-row>
</v-app>
 </template>
 <script>
  import axios from "axios";
  export default {
    name: "App",
    data() {
      return {
        items: [],
        users:null,
    open: ['Users'],
 admins: [
   ['Management', 'mdi-account-multiple-outline'],
   ['Settings', 'mdi-cog-outline'],
 ],
 cruds: [
   ['Create', 'mdi-plus-outline'],
   ['Read', 'mdi-file-outline'],
   ['Update', 'mdi-update'],
   ['Delete', 'mdi-delete'],
 ],
      };
    },
    async created() {
      try {
        var config = {
  method: 'get',
  url: 'https://apitest.nilvera.com/einvoice/Purchase',
  headers: { 
    'Authorization': 'Bearer 0CE1FB33ADAF7E8F90998B1D08B5F94161D049BC2AA5AB299D34DE6ECB1825ED'
  }
};
        const res = await axios(config);
        this.items = res.data;
        console.log( res.data);
      } catch (error) {
        console.log(error);
      }
    },
  };
  </script>
 <!-- <script>
import axios from 'axios';
 export default {
   name: 'GidenKutusu',
   data () {

var config = {
  method: 'get',
  url: 'https://apitest.nilvera.com/einvoice/Sale',
  headers: { 
    'Authorization': 'Bearer 0CE1FB33ADAF7E8F90998B1D08B5F94161D049BC2AA5AB299D34DE6ECB1825ED'
  }
};


axios(config)
.then(function (response) {
  this.users=JSON.stringify(response);
})
.catch(function (error) {
  console.log(error);
});
  return {
    
 
  }
},


 }
 </script> -->