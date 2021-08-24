<template>
     <section id="traffic-light">
        <div :class="{'circle': true, 'red': true, 'opacity': nowColor !== 'red'}"></div>
        <div :class="{'circle': true, 'yellow': true, 'opacity': nowColor !== 'yellow'}"></div>
        <div :class="{'circle': true, 'green': true, 'opacity': nowColor !== 'green'}"></div>
    </section>
</template>

<script>
export default {
    data(){
        return{
            nowColor: 'green',
            prewColor: '',
        }
    },
    methods: {
        interval: function(color){
            if (color === "green") {
                return 7000;
            } else if (color === "yellow") {
                return 2000;
            } else if (color === "red") {
                return 5000;
            }
        },
        timerId: function(currentColor, nextColor){
            let timerId = setInterval(() => {
                this.nowColor = nextColor;
                this.prewColor = currentColor;
            }, this.interval(currentColor));
            setTimeout(() => {
                clearInterval(timerId);
            }, this.interval(currentColor));
        },
        promisse: function(){  
            if (this.nowColor === "green") {
                this.timerId("green", "yellow");
            } else if (this.nowColor === "yellow" && this.prewColor === "green") {
                this.timerId("yellow", "red");
            } else if (this.nowColor === "yellow" && this.prewColor === "red") {
                this.timerId("yellow", "green");
            } else if (this.nowColor === "red") {
                this.timerId("red", "yellow");
            }  
        }
    },
    created: function(){
            this.promisse();    
    },
    watch: {
        nowColor(){
            this.promisse();
        }
    }
}
</script>

<style scoped>
    #traffic-light {
        width: 300px;
        height: 750px;
        background-color: rgba(0, 0, 0, 0.9);
        border: 7px solid black;
        border-radius: 30px;
        margin: 10px auto;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
    }
    .circle {
        width: 200px;
        height: 200px;
        border-radius: 50%;
        border: 5px solid black;
        background: #000;
    }
    .red {
        background: rgba(255, 0, 0, 1);
    }
    .yellow {
        background: rgba(255, 255, 0, 1);
    }
    .green {
        background: rgba(0, 128, 0, 1);
    }
    .opacity {
        opacity: 0.1;
    }
</style>