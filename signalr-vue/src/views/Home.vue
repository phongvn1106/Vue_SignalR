<template>
  <div class="home">
    <span>Trạng Thái : </span>
    <span id="statusId">{{ status }}</span>
    <HelloWorld msg="SignalR Chat App" />
    <div class="table">
      <div class="row">
        <label>User</label>
        <input v-model="user" placeholder="user" />
      </div>
      <div class="row second">
        <label>Message</label>
        <input v-model="message" placeholder="mssage" />
      </div>
      <button class="btn" type="button" v-on:click="SignalR()">
        Submit Chat
      </button>
      <button class="btn second-btn" type="button" v-on:click="Clear()">
        Clear Chat
      </button>
    </div>
    <div v-for="(k, index) in listMessage" :key="index">
      <p>{{ k.name }} : {{ k.message }}</p>
    </div>
    <h3>account _ {{ user }}</h3>
  </div>
</template>

<script lang="ts">
import { Component, Inject, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
import * as signalR from "@aspnet/signalr";
@Component({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
  public status: string = "Chưa login";
  public user: string = "";
  public check: string = "";
  public check2: string = "";
  public message: string = "";
  public listMessage: any = [];

  public connection = new signalR.HubConnectionBuilder()
    // .withUrl('https://localhost:5001/signalr')
    //https://signalv4.cloudlms.top/hubs/authen
    //http://localhost.cloudlms.top:5009
    .withUrl(
      "http://localhost.cloudlms.top:5009/hubs/authen?access_token=eyJhbGciOiJSUzI1NiIsImtpZCI6IjhBNTIxNUQwRjc5MTgzQTlFRDBEMjVBRkQ1REYyRjI5IiwidHlwIjoiYXQrand0In0.eyJuYmYiOjE2NTMzODcyNjAsImV4cCI6MTY1Mzk5MjA2MCwiaXNzIjoiaHR0cDovLzE5Mi4xNjguMS4zODo4MDExIiwiY2xpZW50X2lkIjoiaWRlbnRpdHlTZXJ2aWNlIiwic3ViIjoiNzY5MiIsImF1dGhfdGltZSI6MTY1MzM4NzI2MCwiaWRwIjoibG9jYWwiLCJNYW5hZ2VtZW50SWQiOiIzIiwiT3duZXJJZCI6IjU4NDUiLCJCcmFuY2giOiIiLCJQb3J0YWwiOiJraW5oZG9hbmgiLCJQb3J0YWxJZCI6IjMiLCJEb21haW4iOiJjbG91ZGxtcy5kZXYiLCJMYW5ndWFnZUNvZGUiOiJ2aS12biIsIlRvdGFsQ291cnNlIjoiMTAwIiwiVG90YWxVc2VyIjoiMTAwIiwiTWVtb3J5IjoiMiIsIkZpcnN0TmFtZSI6IjIwMCIsIkxhc3ROYW1lIjoicGhvbmciLCJVc2VyQ29kZSI6IkxIMDI0ODkiLCJQcml2YXRlRG9tYWluIjoiY25hbWUuZWN0YXBwcy5uZXQiLCJTdXBlckFkbWluIjoiRmFsc2UiLCJDb250ZW50TGVhcm4iOiIzIiwiTGVhcm5IaXN0b3J5IjoiMyIsIkxlYXJuUm9hZG1hcCI6IjMiLCJNeUNhbGVuZGVyIjoiMyIsIk15Q291cnNlIjoiMyIsIk15RXhhbU1hbmFnaW5nIjoiMyIsIk15Rm9ydW0iOiIzIiwiTXlMaWJyYXJ5IjoiMyIsIk15U3VydmV5IjoiMyIsIlJvb21PbmxpbmUiOiIzIiwianRpIjoiRENFRkEzNkQ1Q0MyRTE0QUE4NzNEMUFFN0EzRThDN0EiLCJpYXQiOjE2NTMzODcyNjAsInNjb3BlIjpbImVtYWlsIiwib3BlbmlkIiwib2ZmbGluZV9hY2Nlc3MiXSwiYW1yIjpbInB3ZCJdfQ.Bbkbcrrzu5fqvWlT50i150UocHnaHZ7WvEpoCEwwNPIsa1-WZxtC-sfkwyljJB-0SJdmjizd7U1AXp-rnH0bu9B6pT_AEEg_fSdZzZoW3njSJjg7iGgvmf9VGW6ameiS6vdJfAOkTR7jR5ymAk4cSXoqdcdtipIuxaWmyuqaHxwoM-5axt37SmfA5o3x8mNj75lhV9ia3g2wAnChjWCr8HF7QTCvdEBLxDs2Z-XHVuhRVAOThc3ux8y8zbEoeK1bgJ9Xil5JKXF7vuCTF7m5cmXJgO10uacOhby8nFvOmdFVvHsFMvx5ORo_-bNxlP02w2RHHF-mTgcEM_NPzhXImQ"
    )
    .build();
  public mounted() {
      console.log("mounted : ");
    this.connection.on("AnotherUserLogin", (user: string) => {
      //   const insertdata = {name: user, message: "da ok"};
      //    this.listMessage.push(insertdata);
      console.log("Login success : " + user);
    });
  }

  public SignalR() {
     console.log("vao signal");
    if (this.connection.state === signalR.HubConnectionState.Connected) {
      this.connection.invoke("UserLogin");
    } else {
      this.connection.start().then(() => this.connection.invoke("UserLogin"));
        console.log("login ok la");
    }
  }

  public Clear() {
    this.listMessage = [];
  }

  // public meomeo() {
  //     this.status = "Đã login";
  //  console.log("hello");
  // }
}
</script>
<style scoped lang="scss">
.home {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  .table {
    width: 20%;
    height: 50%;
    border: 2px solid black;
    align-self: center;
    padding: 2%;
    .row {
      display: flex;
      flex-direction: row;
      justify-content: center;
      justify-items: center;
    }
    .second {
      margin-top: 4%;
    }
    .btn {
      margin-top: 2%;
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
      width: 40%;
      height: 50%;
      border: 2px solid black;
      align-self: center;
      padding: 2%;
      .row {
        display: flex;
        flex-direction: row;
        justify-content: center;
        justify-items: center;
      }
      .second {
        margin-top: 4%;
      }
      .btn {
        margin-top: 2%;
        cursor: pointer;
      }
      .row label {
        flex: 1;
      }
    }
  }
}
</style>