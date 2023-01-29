<template >
  <ion-page>
    
    <ion-header :translucent="true">
      <ion-toolbar>

 <ion-buttons>
            <ion-back-button default-href="/">Back</ion-back-button>
            <ion-title>Welcome</ion-title>
            </ion-buttons> 


      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
    
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Devinette
            
          </ion-title>
        </ion-toolbar>
      </ion-header>
      <div id="container">
                 <strong>
                  <ion-label> {{comparing}} </ion-label>
                  </strong>
            <br>
               <strong>  
                   
                 <ion-label>You still have {{attemptsRest}} attempts -----{{randomNumber}}   </ion-label><br>
            <ion-label>choose a number between 0 and 100 </ion-label>
             </strong>
           
                 <strong>
            <ion-input   type="number" v-model="choosedNumber"  ></ion-input>
                 </strong>
             
              
                  <strong>
                      <ion-button color="success" @click="play()">Play</ion-button>
                  </strong>
             

          
        
        
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent,alertController ,IonButtons,IonBackButton
    , IonHeader, IonPage,IonLabel,IonInput, IonTitle, IonToolbar,IonButton  } from '@ionic/vue';
import { defineComponent } from 'vue';
import axios from 'axios';


export default defineComponent({
  name: 'Home',
  components: {
    IonContent,IonInput,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonLabel,IonButton  ,IonButtons,IonBackButton
    
  }
 
 ,
  
  data() {
      return {
          attemptsNumber:0,score :0,attemptsRest : 5 , scores:[] ,winnerName :''
        ,randomNumber: Math.floor(Math.random() *100),choosedNumber:0 ,comparing:''

      }
  },
  
  methods: {


async play(){
if ((this.choosedNumber < 0) || (this.choosedNumber > 100) ) {



      const alert = await alertController
        .create({
          cssClass: 'my-custom-class',
          header: 'Shoose a number between 0 and 100',
          
          buttons: ['close'],
        });
      await alert.present();

      const { role } = await alert.onDidDismiss();
      console.log('onDidDismiss resolved with role', role);


}
else{
  
   this.score = (5 - this.attemptsNumber + 1) * 10;


  if (this.choosedNumber != null) {
    
      
      
    
this.attemptsNumber = this.attemptsNumber +1;
this.attemptsRest = this.attemptsRest-1;
  if (this.attemptsNumber == 5) {
      const alert = await alertController
        .create({
          cssClass: 'my-custom-class',
          header: 'GAME OVER',
          subHeader: 'Score',
          message: this.score.toString() ,
          buttons: ['close'],
        });
      await alert.present();

      const { role } = await alert.onDidDismiss();
      console.log('onDidDismiss resolved with role', role);
    }
      else if (this.choosedNumber==this.randomNumber) {
                const alert = await alertController
        .create({
          cssClass: 'my-custom-class',
          header: 'Congratulations',
          subHeader:  'score',
          message: this.score.toString() +'<br>' ,
          inputs: [
        { 
          name: 'winnerName',
          placeholder: 'Write ur full name here',
         
          
        }
      ],
          buttons: [
            {
               text: 'Save',
               handler:(data) => { 
                  this.winnerName = data.winnerName;
                 axios.post("http://localhost:3000/winners",
      {
      fullName:this.winnerName,
      score : this.score
      
     }).then(resp => {
    console.log(resp.data);
}).catch(error => {
    console.log(error);
});         
               
            
          }
              
            },
          ] ,

        });
      await alert.present();

      const { role } = await alert.onDidDismiss();
      console.log('onDidDismiss resolved with role', role);
      }
   
else{
 if (this.choosedNumber > this.randomNumber) {
   this.comparing ='Attempt '+this.attemptsNumber.toString()+ '  :  ' +this.choosedNumber.toString() + '  is superior than golden number'
 } else {
   this.comparing ='Attempt '+this.attemptsNumber.toString()+ '  :  ' +this.choosedNumber.toString() + '  is inferior than golden number'
 }
}



    
  }
  else{
      const alert = await alertController
        .create({
          cssClass: 'my-custom-class',
          header: 'You must enter a number',
          
          buttons: ['close'],
        });
      await alert.present();

      const { role } = await alert.onDidDismiss();
      console.log('onDidDismiss resolved with role', role);
  }
}

}

  
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