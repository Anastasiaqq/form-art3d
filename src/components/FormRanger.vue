<template>
    <div ref="ranger" class="ranger">
        <div class="hot" :style="{width: `${pin}%`}">
            <div class="text text1">{{ text1 }}</div>
        </div>
        
        <div class="pin"
            ref="pin" 
            :style="{left: `calc(${pin}% - 2.5px)`}"
            @mousedown="e => mouseDownHandler(e)"
            >
        </div>
        <div class="cold" :style="{width: `${100 - pin}%`}">
            <div class="text text2">{{ text2 }}</div>
        </div>
        
    </div>
</template>

<script>
export default {
    name: 'FormRanger',
    props: {
        value: {
            type: Number,
            default: 10
        },
        text1: {
            type: String,
            default: 'from'
        },
        text2: {
            type: String,
            default: 'to'
        }
    },
    data() {
        return {
            clicked: false,
            pin: this.value,
            containerLeft: undefined,
            rangerWidth: undefined
        }
    },
    mounted() {
        this.containerLeft = this.$refs.ranger.offsetLeft
        let rangerstyle = window.getComputedStyle(this.$refs.ranger).width
        this.rangerWidth = parseInt(rangerstyle.slice(0, -2))
    },
    methods: {
        mouseDownHandler(e) {
            document.onmousemove = this.moveHandler.bind(this)
            document.onmouseup = this.clickEndHandler.bind(this)
            document.onmouseleave = this.clickEndHandler.bind(this)
            document.onmouseout = this.moveHandler.bind(this)
            this.clicked = true
            e.preventDefault()
        },
        moveHandler(e) {
            this.pin =this.value
            if(this.clicked) {
                let {clientX} = e
                if(clientX > this.containerLeft && clientX < this.containerLeft + this.rangerWidth) {
                    this.pin = (((clientX - this.containerLeft)/this.rangerWidth)*100)
                }   
                else if(clientX <= this.containerLeft) {
                    this.pin = 0
                }
                else {
                    this.pin = 100
                }
                this.$emit('input', this.pin)
                this.$emit('change', this.pin)
            }
        },
        clickEndHandler() {
            this.clicked = false
            document.onmousemove = null
            document.onmouseup = null
            document.onmouseleave = null
        },
    }
}
</script>
<style scoped>
.ranger {
    position: relative;
    width: 100%;
    height: 1px;
    background: grey;
}
.pin {
    position: absolute;
    top: -6px;
    width: 13px;
    height: 13px;
    background: #B6A16B;
    border-radius: 50%;
    cursor: pointer;
    z-index: 100;
}
.hot {
    position: absolute;
    top:0;
    left: 0;
    height: 1px;
    background-color: #FF69B4;
}
.text {
    color: #EEEBE6;
    font-style: normal;
    font-weight: 400;
    font-size: 9px;
    letter-spacing: 0.05em;
    text-transform: uppercase;
}
.text1 {
    position:absolute;
    top: 11px;
    left: 4px;
}
.cold {
    position: absolute;
    top:0;
    right: 0;
    height: 1px;
    background-color: #5BAAF9;
}
.text2 {
    position: absolute;
    top: 11px;
    right: 2px;
}
</style>