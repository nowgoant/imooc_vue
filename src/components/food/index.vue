<template>
    <transition name="move">
        <div v-show="showFlag" class="food" ref="food">
            <div class="foo-content">
                <div class="image-header">
                    <img :src="food.image" alt="">
                    <div class="back" @click="hide">
                        <i class="icon-arrow_lift"></i>
                    </div>
                </div>
                <div class="content">
                    <h1 class="title">{{food.name}}</h1>
                    <div class="detail">
                        <span class="sell-count">月售{{food.sellCount}}份</span>
                        <span class="rating">好评率{{food.rating}}%</span>
                    </div>
                    <div class="price">
                        <span class="now">￥{{food.price}}</span>
                        <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                    </div>
                    <div class="cartcontrol-warpper">
                        <cartcontrol :food="food" v-on:cardadd="_cardadd"></cartcontrol>
                    </div>
                    <transition name="fade">
                        <div class="buy" @click.stop.prevent="addFirst" v-show="!food.count || food.count===0">加入购物车</div>
                    </transition>
                </div>
                <split></split>
                <div class="info" v-show="food.info">
                    <h1 class="title">商品信息</h1>
                    <p class="text">{{food.info}}</p>
                </div>
                <split></split>
    
            </div>
        </div>
    </transition>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue'
import BScroll from 'better-scroll'
import cartcontrol from '@/components/cartcontrol'
import split from '@/components/split'

export default {
    props: {
        food: {
            type: Object
        }
    },
    components: {
        cartcontrol,
        split
    },
    data() {
        return {
            showFlag: false
        }
    },
    methods: {
        show() {
            this.showFlag = true
            this.$nextTick(() => {
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$refs.food, {
                        click: true
                    })
                } else {
                    this.scroll.refresh()
                }
            })
        },
        hide() {
            this.showFlag = false
        },
        addFirst(event) {
            if (!event._constructed) {
                return
            }
            this._cardadd(event.target)
            Vue.set(this.food, 'count', 1)
        },
        _cardadd(target) {
            this.$emit('cardadd', target)
        }
    }
}
</script>

<style lang="scss" rel="stylesheet/scss">
.food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background: #fff;
    transform: translate3d(0, 0, 0);
    &.move-enter-active,
    &.move-leave-active {
        transition: all 0.2s linear;
    }
    &.move-enter,
    &.move-leave-active {
        transform: translate3d(100%, 0, 0);
    }

    .image-header {
        position: relative;
        width: 100%;
        height: 0;
        padding-top: 100%;
        img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        .back {
            position: absolute;
            top: 10px;
            left: 0;
            .icon-arrow_lift {
                display: block;
                padding: 10px;
                font-size: 20px;
                color: #fff;
            }
        }
    }
    .rating {
        padding-top: 18px;
    }
    .content {
        position: relative;
        padding: 18px;
        .title {
            line-height: 14px;
            margin-bottom: 8px;
            font-size: 14px;
            font-weight: 700;
            color: #07111b;
        }
        .detail {
            margin-bottom: 18px;
            line-height: 10px;
            height: 10px;
            font-size: 0;
            .rating,
            .sell-count {
                font-size: 10px;
                color: #93999f;
            }
            .sell-count {
                margin-right: 12px;
            }
        }
        .price {
            font-weight: 700;
            line-height: 24px;
            .now {
                margin-right: 8px;
                font-size: 14px;
                color: #f01414;
            }
            .old {
                text-decoration: line-through;
                font-size: 10px;
                color: #93999f;
            }
        }
        .cartcontrol-warpper {
            position: absolute;
            right: 12px;
            bottom: 12px;
        }
        .buy {
            position: absolute;
            right: 18px;
            bottom: 18px;
            z-index: 10;
            height: 24px;
            line-height: 24px;
            padding: 0 12px;
            box-sizing: border-box;
            border-radius: 12px;
            font-size: 10px;
            color: #fff;
            background: #00a0dc;

            &.fade-enter-active,
            &.fade-leave-active {
                transition: opacity 0.5s;
            }
            &.fade-enter,
            &.fade-leave-active {
                opacity: 0;
            }
        }
    }
    .info {
        padding: 18px;
        .title {
            line-height: 14px;
            margin-bottom: 6px;
            font-size: 14px;
            color: #07111b;
        }
        .text {
            line-height: 24px;
            padding: 0 8px;
            font-size: 12px;
            color: #4d555d;
        }
    }
}
</style>
