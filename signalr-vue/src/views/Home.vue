<template>
  <div class="home">
    <span>Trạng Thái : </span>
    <span id="statusId">{{status}}</span>
    <HelloWorld msg="SignalR Chat App"/>
    <div class="table">
    <div class="row">
      <label>User</label>
      <input v-model="user" placeholder="user">
    </div>
    <div class="row second">
      <label>Message</label>
      <input v-model="message" placeholder="mssage">
    </div>
    <button class="btn" type="button" v-on:click="SignalR()">Submit Chat</button>
    <button class="btn second-btn" type="button" v-on:click="Clear()">Clear Chat</button>
     </div>
    <div v-for="(k,index) in listMessage" :key="index">
      <p>{{k.name}} : {{k.message}}</p> 
    </div>  
 <h3>account _ {{user}}</h3>

  </div>

  
</template>

<script lang="ts">
import { Component, Inject, Vue } from 'vue-property-decorator';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src
import * as signalR from '@aspnet/signalr';
@Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {

public status: string = 'Chưa login';
  public user: string = '';
    public check: string = '';
    public check2:string = '';
  public message: string = '';
  public listMessage: any = [];
 
  public  connection = new signalR.HubConnectionBuilder()
 // .withUrl('https://localhost:5001/signalr')
   .withUrl('https://signalv4.cloudlms.top/hubs/authen')
    .build();
  public mounted() {
    this.connection.on('ReceiveMessage', (user: string, data: string) => {
           const insertdata = {name: user, message: data};
           this.listMessage.push(insertdata);
          console.log ("Login success : " + user);
              
     });
  }
  public SignalR() {
    if (this.connection.state === signalR.HubConnectionState.Connected) {
    this.connection.invoke('SendMessage', this.user, this.message);
        } else {
           
    this.connection.start()
            .then(() => this.connection.invoke('SendMessage', this.user, this.message));
       //     console.log("login ok la");
          
      }
  }



public Clear() {
  this.listMessage = [];
}

  public meomeo() {
      this.status = "Đã login";
   console.log("hello");
  }
}
</script>
<style scoped lang="scss">
.home {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  .table {
    width : 20%;
    height: 50%;
    border:2px solid black;
    align-self: center;
    padding: 2%;
    .row {
      display: flex;
      flex-direction: row;
      justify-content: center;
      justify-items: center;
    }
     .second {
        margin-top:4%;
      }
      .btn {
        margin-top:2%;
        cursor: pointer;
      }
      .second-btn {
        margin-left: 2%;
      }
    .row label {
      flex: 1;
    }
  }
}

@media (min-width: 400px) and (max-width: 1024px) {
.home {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  .table {
    width : 40%;
    height: 50%;
    border:2px solid black;
    align-self: center;
    padding: 2%;
    .row {
      display: flex;
      flex-direction: row;
      justify-content: center;
      justify-items: center;
    }
     .second {
        margin-top:4%;
      }
      .btn {
        margin-top:2%;
        cursor: pointer;
      }
    .row label {
      flex: 1;
    }
  }
}
}

</style>