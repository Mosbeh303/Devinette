<template>
  <ion-page>
    
    <ion-header :translucent="true">
      <ion-toolbar>
        
           <ion-buttons>
            <ion-back-button default-href="/">Back</ion-back-button>
            <ion-title>Winners</ion-title>
            </ion-buttons> 
            
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      
      
    
 
        <strong>         
                              <ion-list>
                                  <ion-item  v-for="winner in winners" :key="winner.score"   class="list-item">
                                      
                                           <ion-label >{{winner.fullName}} : {{winner.score}}</ion-label>
                                    
                                  </ion-item>
                              </ion-list>
        </strong>
    
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonBackButton,IonButtons,IonHeader,IonLabel, IonPage, IonTitle, IonToolbar,IonList,IonItem } from '@ionic/vue';
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonList,IonItem,IonLabel,IonBackButton,IonButtons
    
  },
  data() {
      return {
          winners:[],
      }
  },
  
  
  created() {


    axios.get("http://localhost:3000/winners?_sort=score&_order=desc").then(response=>{this.winners=response.data});
   


  },
});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>