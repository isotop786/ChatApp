<template>
    <app-layout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Chat
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white p-3 overflow-hidden shadow-xl sm:rounded-lg">
                    {{rooms}}
                    <Message/>
                    <inputMessage/>
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

    export default {
        components: {
            AppLayout,
            Message,
            inputMessage
        },
        data(){
            return{
                rooms : []
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

                })
                .catch(err=>{
                    console.log(err)
                })
            }
        }
    }
</script>
