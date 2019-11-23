<template>
    <div class="codemaster-ui" :class="turn">
        <template v-if="select!=='word'">
            <div class="codemaster-ui__count count">
                <div class="count__text">{{t('How many cards')}}?</div>
                <div class="count__input">
                    <button class="count__button count__button_remove"
                        :disabled="count===1"
                     @click="decreaseCount">-</button>
                    <span class="count__number">{{count}}</span>
                    <button class="count__button count__button_add"
                    :disabled="count===max"
                     @click="increaseCount">+</button>
                </div>
            </div>
        </template>
    </div>
</template>

<script>
    import {eventBus} from '../../main';
    export default {
        props: {
            disabled: Boolean,
            turn: String,
            word: String,
            select: String,
            count: Number,
            max: Number,
        },
        data() {
            return {
                keys: [{row: ["q", "w", "e", "r", "t", "y", "u", "i", "o", "p"]},{
                        row: ["a", "s", "d", "f", "g", "h", "j", "k", "l"]}, {
                        row: ["z", "x", "c", "v", "b", "n", "m", "delete"]},{
                        row: [",",".","_"]},{
                        row: ["space"]}]
            }
        },
        methods: {
            vibrate(milisseconds) {
                navigator.vibrate = navigator.vibrate || navigator.webkitVibrate || navigator.mozVibrate || navigator.msVibrate;
                if (navigator.vibrate) {
                    navigator.vibrate(milisseconds);
                }
            },
            decreaseCount() {
                this.vibrate(20);
                eventBus.$emit('codeChanged',{
                    word: this.word,
                    select: this.select,
                    count: this.count > 1 ? this.count - 1 : this.count
                });
            },
            increaseCount() {
                this.vibrate(20);
                eventBus.$emit('codeChanged',{
                    word: this.word,
                    select: this.select,
                    count: this.count < this.max ? this.count + 1 : this.count
                });
            }
        },
        mounted() {
            if (this.$translate.lang==='pt_br') {
                this.keys[1].row.push('ç');
            }
        },
        locales: {
            pt_br: {
                'How many cards' : 'Quantas cartas'
            }
        }
    }
</script>