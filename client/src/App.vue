<template>
<div id="chatContainer">


   <div class="chatHeader">
     <div class="container">
    
     <img src="https://img.freepik.com/free-vector/businessman-character-avatar-isolated_24877-60111.jpg?w=2000" alt="Avatar" class="avatar">
     <h5>Chat Bot</h5>
     </div>
    </div>
    <div class="chatBody" id="chatbody">
      <div class="messages" v-for="message in messages" :key="message.id">
        <div class="messageRow user" v-if="message.id % 2 == 0">
          <div class="message user">
            <p>{{ message.message }}</p>
          </div>
        </div>
        <div class="messageRow bot" v-else>
          <div class="message bot">
            <p>{{ message.message }}</p>
          </div>
        </div>
      </div>
    </div>
    <!-- <div class="chatFooter">
      <form @submit.prevent="sendMessage()">
        <input v-model="messageContent" id="createMessage" />
        <input  class="submit" type="submit" />
      </form>
    </div>  -->
    <div class="container">
	<div class="container__item">
		<form class="form" @submit.prevent="sendMessage()">
			<input v-model="messageContent"   type="text" class="form__field" placeholder="Enter Text...." />
			<button  type="submit" class="btn btn--primary btn--inside uppercase">Send</button>
		</form>
	</div>
	
	
</div>
  </div> 
  
</template>

<script>
window.setInterval(() => {
  const elem = document.getElementById("chatbody");
  elem.scrollTop = elem.scrollHeight;
}, 100);
import axios from "axios";
import { ref } from "vue";
export default {
  name: "App",

  setup() {
    const messages = ref([]),
      messageContent = ref("");

    //Sends the message on form submit
    function sendMessage() {
      if (messageContent.value == "") return;
      createMessage(messageContent.value);
      getResponse(messageContent.value);
      messageContent.value = "";
       
       var objDiv = document.getElementById("message1");
objDiv.scrollTop = objDiv.scrollHeight
    }

    //Create a message
    function createMessage(message) {
      let id = 0;
      if (messages.value[messages.value.length - 1]) {
        id = messages.value[messages.value.length - 1].id + 1;
      }
      messages.value.push({
        id: id,
        message: message,
      });
     
    }

    async function getResponse(message) {
      const postData = {
        message: message,
      };
      const { data } = await axios.post("http://localhost:8000/chat", postData);
      const { response } = data;
      createMessage(response);
    }

    return { messages, messageContent, sendMessage };
  },
};
</script>

<style>
.avatar {
  vertical-align: middle;
  width: 50px;
  height: 50px;
  border-radius: 50%;
}
#chatContainer {
  background-image: url("https://cdn.wallpapersafari.com/46/78/exDpS7.jpg");
  height: 600px;
  width: 40%;
  
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  position: relative;
}
.chatHeader {
  box-shadow: 0 4px 8px 0 rgba(97, 58, 58, 0.2);
  color: white;
  margin-left: 5%;
}
.chatFooter {
  position: absolute;
  bottom: 0px;
  width: 100%;
}
.chatFooter form {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  width: 95%;
  margin: 0 auto;
}
.chatBody {
  overflow-y: auto;
  height:90%;
}
#createMessage {
  width: 80%;
}
input:not(#createMessage) {
  background-color: rgba(255, 255, 255, 0.5);
  border: 20px;
  color: rgb(22, 3, 3);
  padding: 10px;
  font-size: 20px;
  margin-bottom: 12px;
  opacity: 0.8;
}
input:not(#createMessage):hover {
  opacity: 0.5;
}
.messageRow {
  display: flex;
  justify-content: flex-end;
}
.messageRow.bot {
  justify-content: flex-start;
}
.message p {
  color: white;
  padding: 0px 15px 0px 15px;
}
.message {
  border-radius: 50px;
  text-align: center;
  margin: 10px;
}
.messageRow.user .message {
  background-color: rgba(158, 172, 172, 0.5);
  
  margin-right: 10px;

}
.messageRow.bot .message {
  background-color: rgba(170, 188, 212, 0.5);
  margin-left: 10px;
}
.chatBody::-webkit-scrollbar {
  width: 0px;
  height: 100%;
}
.submit{
   background: #0d6102;
color: rgb(22, 2, 2);
border: 1px solid rgb(24, 3, 3);
border-radius: 20px;
box-shadow: 5px 5px 5px rgb(8, 31, 241);
text-shadow: none;
}
.submit:hover {
background: #056e0e;
color: rgb(14, 1, 1);
border: 1px solid #eee;
border-radius: 20px;
box-shadow: 5px 5px 5px #eee;
text-shadow: none;
}



.container {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.uppercase {
	text-transform: uppercase;
}


.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.uppercase {
  text-transform: uppercase;
}

.btn {
  display: inline-block;
  background: transparent;
  color: inherit;
  font: inherit;
  border: 0;
  outline: 0;
  padding: 0;
  transition: all 200ms ease-in;
  cursor: pointer;
}
.btn--primary {
  background: #7f8ff4;
  color: #fff;
  box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.1);
  border-radius: 2px;
  padding: 12px 36px;
}
.btn--primary:hover {
  background: #6c7ff2;
}
.btn--primary:active {
  background: #7f8ff4;
  box-shadow: inset 0 0 10px 2px rgba(0, 0, 0, 0.2);
}
.btn--inside {
  margin-left: -20px;
}

.form__field {
  width: 240px;
  height: 30px;
  background: #fff;
  font: #070000;
  font: inherit;
  box-shadow: 0 6px 10px 0 rgba(0, 0, 0, 0.1);
  border: 0;
  outline: 0;
  padding: 22px 18px;
}
</style>
