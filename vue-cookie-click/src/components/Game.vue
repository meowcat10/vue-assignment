<script>
import defaultCookie from './icons/defaultCookie.png';
import whiteCookie from './icons/whiteCookie.png';
import smartCookie from './icons/smartCookie.png';

export default {
    name: 'Game',
    components: {  },
    data() {
        return { 
            currentSkin:0,
            cookieSkins:[
                { src: defaultCookie, index: 0 },
                { src: whiteCookie, index: 1 },
                { src: smartCookie, index: 2 }
            ]
        }
    },
    props: { 
        clickRatio: { type:Number, required: true },
        clicks: { type:Number, required:true },
        cookieSkin: { type: Number, required: true}
    },
    computed: {
        rotationDuration(){
            // Spins faster as clicks go up, capped at a minimum duration
            return Math.max(0.2, 5 - this.clicks / 20) + 's';
        }
    },
    methods: { },
    watch: { 
        cookieSkin(newVal){
            this.currentSkin = newVal;
        }
    },
    emits: ['user-click'],
}
</script>

<template>
    <main>
        <h3>{{ clicks }}</h3>
        <p>{{ clickRatio }} Cookies pr click!</p>
        <img
            @cookieChange="changeSkin($event)"
            @click="this.$emit('user-click')"
            :style="{ animationDuration: rotationDuration }"
            :src="this.cookieSkins[this.currentSkin].src"
            alt="Cookie">
    </main>
</template>

<style scoped>
    main {
        display:flex;
        flex-direction:column;
        justify-content: center;
        align-items: center;
        background:rgba(255, 255, 255, 0.547);

        p{
            padding:0; margin:0;
            font-size:6px;
        }

        img{
            max-width:275px;
            max-height:275px;
            animation: rotate_cookie 5s infinite linear;
            transition: all 0.5s;
            
            &:hover{
                cursor:pointer;
            }
        }
    }


</style>