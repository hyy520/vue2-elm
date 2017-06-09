<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">
                {{desc.all}}
                <span
        class="count">{{ratings.length}}</span>
            </span>
            <span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">
                {{desc.positive}}
                <span
        class="count">{{positives.length}}</span>
            </span>
            <span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">
                {{desc.negative}}
                <span
        class="count">{{negatives.length}}</span>
            </span>
        </div>
        <div @click="toggleContent" class="switch" :class="{'on':onlyContent}">
            <span class="icon-check_circle"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
    const POSITIVE = 0;
    const NEGATIVE = 1;
    const ALL = 2;

    export default {
        props: {
            ratings: {
                type: Array,
                default() {
                    return [];
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
                    };
                }
            }
        },
        computed: {
            positives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === POSITIVE;
                });
            },
            negatives() {
                return this.ratings.filter((rating) => {
                    return rating.rateType === NEGATIVE;
                });
            }
        },
        methods: {
            select(type, event) {
                if (!event._constructed) {
                    return;
                }
                // this.selectType = type; ==》(vue1的做法) 不建议直接在子组件直接修改父组件的prop，这样状态会不可控
                // this.$dispatch('ratingtype.select', type);
                this.$emit('selectType', type);  // 通过$emit把事件派发出去
            },
            toggleContent(event) {
                if (!event._constructed) {
                    return;
                }
                this.$emit('content');
            }
        }
    };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "./ratingselect.styl";
</style>