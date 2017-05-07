<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease " @click.stop.prevent="decreaseCart($event)" v-show="food.count>0">
                <div class="inner icon-remove_circle_outline"></div>
            </div>
        </transition>
        
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add icon-add_circle" @click.stop.prevent="addCart($event)"></div>
    </div>
</template>
<script>
    import Vue from 'vue'
    export default {
        props: {
            food: {
                type: Object
            }
        },
        created() {
            // console.log(this.food)
        },
        methods: {
            addCart(e) {
                if (!e._constructed) {
                    return 
                }
                if(!this.food.count) {
                    // this.food.count = 1
                    Vue.set(this.food, 'count', 1)
                } else {
                    this.food.count += 1
                }

	            this.$root.eventHub.$emit('cart.add', e.target)
            },
            decreaseCart(e) {
                if (!e._constructed) {
                    return 
                }
                if (this.food.count) {
                    this.food.count --
                }
            }

        }
    }
</script>
<style lang="stylus" rel="stylesheet/stylus">
    .cartcontrol
        font-size: 0
        .cart-decrease,.cart-add
            display: inline-block
            padding: 6px
            transition: all 0.4s linear
            .inner
                display: inline-block
                line-height: 24px
                font-size: 24px  
                color: rgb(0,160,220)
                transition: all 0.4s linear
                transform: rotate(0)
            &.move-enter, &.move-leave-active
                opacity: 0
                transform: translate3d(24px, 0 , 0)
                .inner 
                    transform: rotate(180deg)
        .cart-count
            display: inline-block
            width: 12px
            vertical-align: top
            padding-top: 6px
            line-height: 24px
            text-align: center
            font-size: 10px
            color: rgb(147, 153, 159)
        .cart-add
            display: inline-block
            padding: 6px
            line-height: 24px
            font-size: 24px  
            color: rgb(0,160,220)
                      
</style>