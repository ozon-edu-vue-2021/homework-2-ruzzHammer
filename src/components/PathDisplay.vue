<template>
    <div class="display">
        {{path}}
    </div>
</template>

<script>
export default {
    name: 'PathDisplay',
    props: {
        path: String
    },
    mounted() {
        const display = this.$el;
        window.addEventListener('scroll', () => {
            if (window.scrollY > display.clientHeight) display.classList.add('fixed')
            else display.classList.remove('fixed')
        })
    }
}
</script>

<style scoped>
.display {
    background: #3b3b3b;
    color: #fff;
    width: 100%;
    padding: 15px;
    text-align: center;
}
.display.fixed {
    position: fixed;
    top: 0;
    left: 0;
    box-shadow: 5px 5px 10px rgba(105, 105, 105, 0.534);
    animation: helloThere .25s ease;
}
@keyframes helloThere {
    0% {
        top: -100%;
    }
    100% {
        top: 0;
    }
}
</style>
