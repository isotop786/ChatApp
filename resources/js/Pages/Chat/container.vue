<template>
    <app-layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
              <chatRoomSelection
                v-if="currentRoom.id"
                :rooms="rooms"
                :currentRoom="currentRoom"
                @roomChanged="setRoom($event)"
              />
            </h2>
        </template>

        <div class="py-12">

            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">

                <div class="bg-white p-3 overflow-hidden shadow-xl sm:rounded-lg">

                   <span v-if="rooms.length">
                       <span v-for="r in rooms" :key="r.id">
                           {{r}}
                       </span>
                   </span>
                   <template v-if="messages">

                              <Message :messages="messages" />


                   </template>

                    <inputMessage @message-sent="getMessages()" :room="currentRoom"/>
                </div>
            </div>
        </div>
    </app-layout>
</template>

<script>
    import axios from 'axios'
    import AppLayout from '@/Layouts/AppLayout'
    import Message from './messageContainer';
    import inputMessage from './inputMessage';
    import chatRoomSelection from './chatRoomSelection';

    export default {
        components: {
            AppLayout,
            Message,
            inputMessage,
            chatRoomSelection
        },
        data(){
            return{
                rooms : [],
                currentRoom:[],
                messages:[]
            }
        },
        created(){
            this.getRooms();

        },
        methods:{
            getRooms(){
                axios.get('/chat/rooms')
                .then(res=>{
                    this.rooms = res.data
                    this.setRoom(res.data[0])
                    this.getMessages()
                })
                .catch(err=>{
                    console.log(err)
                })
            },

            setRoom(room){
                this.currentRoom = room
            },
            getMessages(){
                axios.get(`/chat/room/${this.currentRoom.id}/messages`)
                .then(res=>{
                    this.messages = res.data
                })
                .catch(err=>{
                    console.log(err)
                })
            }
        }
    }
</script>
