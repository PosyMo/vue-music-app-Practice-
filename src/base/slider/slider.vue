<template>
    <div class="slider" ref="slider">
        <div class="slider-group" ref="sliderGroup">
            <slot>
            </slot>
        </div>
        <div class="dots">
            <span class="dot" :class="{ active: currentPageIndex === index }" v-for="(item, index) in dots" :key="index"></span>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    import {addClass} from 'common/js/dom'
    import BScroll from 'better-scroll'
    
    export default {
        name: 'slider',
        props: {
            loop: {
                type: Boolean,
                default: true
            },
            autoplay: {
                type: Boolean,
                default: true
            },
            interval: {
                type: Number,
                default: 4000
            }
        },
        data() {
            return {
                dots: [],
                currentPageIndex: 0
            }
        },
        mounted() {
            setTimeout(() => {
                this._setSlideWidth()
                this._initDots()
                this._initSlider()

                if (this.autoplay) {
                  this._play()
                }
            }, 20)

            window.addEventListener('resize', () => {
                if (!this.slider) {
                    return
                }
                this._setSlideWidth(true)
                this.slider.refresh()
            })
        },
        activated() {
            if (this.autoplay) {
                this._play()
            }
        },

        deactivated() {
            clearTimeout(this.timer)
        },
        destroyed() {
            clearTimeout(this.timer)
        },
        methods: {
            _setSlideWidth(isRefresh) {
                //children这类公用属性，直接绑定在this实例上
                this.children = this.$refs.sliderGroup.children

                let width = 0
                let sliderWidth = this.$refs.slider.clientWidth
                for(let i = 0; i < this.children.length; i++) {
                    let child = this.children[i]
                    addClass(child, 'slider-item')

                    child.style.width = sliderWidth + 'px'
                    width += sliderWidth
                }

                if (this.loop && !isRefresh) {
                    width += sliderWidth * 2
                }

                this.$refs.sliderGroup.style.width = width + 'px'
            },
            _initSlider() {
                //使用官方demo中的方式会报错
                // snap: {
                //     loop: this.loop,
                //     threshold: 0.3,
                //     speed: 400
                // }
                this.slider = new BScroll(this.$refs.slider, {
                    scrollX: true,
                    scrollY: false,
                    momentum: false,
                    snap: true,
                    snapLoop: this.loop,
                    snapThreshold: 0.3,
                    snapSpeed: 400
                })

                this.slider.on('scrollEnd', () => {
                    let pageIndex = this.slider.getCurrentPage().pageX
                    if (this.loop) {
                        pageIndex -= 1
                    }
                    this.currentPageIndex = pageIndex

                    if (this.autoplay) {
                        this._play()
                    }
                })

                this.slider.on('beforeScrollStart', () => {
                    if (this.autoplay) {
                        clearTimeout(this.timer)
                    }
                })
            },
            _initDots() {
                this.dots = new Array(this.children.length)
            },
            _play() {
                let pageIndex = this.currentPageIndex + 1
                if (this.loop) {
                    pageIndex += 1
                }
                this.timer = setTimeout(() => {
                    this.slider.goToPage(pageIndex, 0 ,400)
                }, this.interval)
            }
        }
    }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
    @import "~common/stylus/variable"
    
    .slider
        position relative
        min-height 1px
        overflow hidden
        .slider-group
            position relative
            overflow hidden
            white-space nowrap
            .slider-item
                float left
                box-sizing border-box
                overflow hidden
                a
                    display block
                    width 100%
                    overflow hidden
                    text-decoration none
                img
                    display block
                    width 100%
        .dots
            position absolute
            right 0
            left 0
            bottom 8px
            font-size 0
            text-align center
            .dot
                margin 0 3px
                width 6px
                height 6px
                display inline-block
                border-radius 50%
                background $color-text-d
                &.active
                    background $color-theme
                    opacity .8
</style>
