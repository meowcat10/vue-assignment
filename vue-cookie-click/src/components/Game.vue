<script>
export default {
    name: 'Game',
    components: {  },
    data() {
        return { }
    },
    props: { 
        clickRatio: { type:Number, required: true },
        clicks: { type:Number, required:true }
    },
    computed: { },
    methods: { },
    watch: { },
    emits: ['user-click'],
}
</script>

<template>
    <main>
        <h3>{{ clicks }}</h3>
        <p>{{ clickRatio }} Cookies pr click!</p>
        <img @click="this.$emit('user-click')" src="./icons/pngtree-chocolate-chip-cookie-icon-white-background-png-image_16100260.png" alt="Cookie">
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
            max-width:300px;
            max-height:300px;
            animation: rotate_cookie 3s infinite linear;
        }
    }


</style>