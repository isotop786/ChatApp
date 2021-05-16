<template>
  <div class="relative h-10 m-1">
      <div style="border-top: 1px solid #e6e6e6"
        class="grid grid-cols-6"
      >
          <input type="text" style="border:none"
          class="col-span-5 p-1 focus:outline-none focus:ring focus:border-blue-300"
          v-model="message"
          @keyup.enter="sendMessage()"
          placeholder="Say something..."
          >
          <button class="place-self-end bg-gray-500 hover:bg-blue-700 p-1 mt-1 rounded text-white"
            @click.prevent="sendMessage()"
            :disabled="!message"
          >
                send
          </button>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
    props: ['room'],
   data(){
       return{
           message:''
       }
   },
   methods:{
       sendMessage(){
           if(this.message == ' '){
               return;
           }

           axios.post(`/chat/room/${this.room.id}/message`,{
               message:this.message
           })
           .then(res=>{
              if(res.status == 200){
                  this.message = '';
                  this.$emit('message-sent')
              }
           })
           .catch(err=>{
               console.log(err)
           })
       }
   },
   created(){
       console.log("the room is "+this.room.user_id)
   }


}
</script>

<style>

</style>
