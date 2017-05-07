<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span @click="select(2,$event)" class="block positive"
                :class="{'active': myselectType===2}">{{desc.all}}<span
                class="count">{{ratings.length}}</span></span>
            <span @click="select(0,$event)"  class="block positive" 
                :class="{'active': myselectType===0}">{{desc.positive}}<span
                 class="count">{{positive.length}}</span></span>
            <span @click="select(1,$event)"  class="block negative" 
                :class="{'active': myselectType===1}">{{desc.negative}}<span
                 class="count">{{negatives.length}}</span></span>
        </div>
        <div class="switch" :class="{'on': myonlyContent}" @click="toggleContent($event)">
            <span class="icon-check_circle"></span>
            <span class="only">只看有内容的评价</span>
        </div>
    </div>
</template>
<script>
    const POSITIVE = 0
    const NEGATIVE = 1
    const ALL = 2
	
    export default {
        props: {
            ratings: {
                type: Array,
                default() {
                    return []
                }
            },
            selectType: {
                type: Number,
                default: ALL
            },
            onlyContent: {
                type: Boolean,
                default: false
            },
            desc: {
                type: Object,
                default() {
                    return {
                        all: '全部',
                        positive: '满意',
                        negative: '不满意'
                    }
                }
            }
        },
        data() {
            return {
                //外部props副本，vue 2.x 变为单向数据流动，所以通过缓存副本的方式向外回传
                myselectType: this.selectType,
                myonlyContent: this.onlyContent
            }
        },
        computed: {
            positive() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === POSITIVE
                })
            },
            negatives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === NEGATIVE
                })
            }
        },
        methods: {
            select(type, e) {
                if (!e._constructed) {
                    return
                }
                this.myselectType = type
                this.$root.eventHub.$emit('ratingtype.select', type)
            },
            toggleContent(e) {
                if (!e._constructed) {
                    return
                }
                this.myonlyContent = !this.myonlyContent
                this.$root.eventHub.$emit('content.toggle', this.myonlyContent)                
            }
        }
    }
</script>
<style lang="stylus" rel="stylesheet/stylus">
    @import '../../common/stylus/mixin.styl'
    .ratingselect
        .rating-type
            padding: 18px 0
            margin: 0 18px
            border-1px(rgba(7,17,27,.1))
            font-size: 0
            .block
                display: inline-block
                padding: 8px 12px
                margin-right: 8px
                line-height: 16px
                border-radius: 1px
                color:rgb(77,85,93)
                font-size: 12px
                &.active
                    color: #fff
                .count
                    font-size: 8px
                    margin-left: 2px
                &.positive
                    background: rgba(0,160,220, 0.2)
                    &.active
                        background: rgb(0,160,220)
                &.negative
                    background: rgba(77,85,93,0.2)
                    &.active
                        background: rgb(77,85,93)
        .switch
            padding: 12px 18px
            line-height: 24px
            color: rgb(147,153,159)
            border-bottom: 1px solid rgba(7,17,27,.1)
            font-size: 0
            &.on 
               .icon-check_circle
                    color: #00c850
            .icon-check_circle
                display:inline-block
                vertical-align: top
                font-size:24px
                margin-right: 4px
            .only 
                display:inline-block
                vertical-align: top
                font-size: 12px
                

</style>