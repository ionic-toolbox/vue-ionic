<template>
    <div class="ion-action-sheet " :class="['action-sheet-'+theme]">
`        <ion-backdrop @click.native="onClose"  ref="backdrop"></ion-backdrop>
        <transition name="ion-action-sheet-fade">
            <div v-show="currentValue" class="action-sheet-wrapper">
                <div class="action-sheet-container">
                    <div class="action-sheet-group">
                        <div class="action-sheet-title">{{ title }}</div>
                        <ion-button v-for="item in buttons" :class="[item.type === 'del'?'action-sheet-destructive':'']" @click.native.stop="itemClick(item)" prefix="action-sheet-button"> {{ item.text }} 
                        </ion-button>
                    </div>
                    <div class="action-sheet-group" >
                        <ion-button @click.native="onClose" prefix="action-sheet-button" class=" action-sheet-icon">cancel</ion-button>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>
<script>
    import ThemeMixin from 'src/minins/theme.mixins';
    import ionBackdrop from 'src/components/backdrop';
    import ionButton from 'src/components/button';
    export default {
        name: 'ion-action-sheet',
        mixins: [ThemeMixin],
        data() {
            return {
                currentValue: false
            }
        },
        watch: {
            currentValue(val) {
                this.$emit('input', val);
            },
            value(val) {
                this.currentValue = val;
            }
        },
        methods: {
            open() {
                this.$refs.backdrop.setStyle('opacity', '0.4');
                this.$refs.backdrop.disableClick();
                this.currentValue = true;
                setTimeout(() => {
                    this.$refs.backdrop.enableClick();
                }, 400);
            },
            onClose() {
                this.currentValue = false;
                setTimeout(() => {
                    this.$refs.backdrop.setStyle('opacity', '0.01');
                    this.$el.remove();
                }, 350);

            },
            itemClick(item) {
                if (item.handler && typeof item.handler === 'function') {
                    item.handler();
                }
                this.onClose();
            }
        },
        props: {
            value: {}
        },
        components: {
            ionBackdrop,
            ionButton
        }
    };
</script>
<style lang="scss">
    @import './action-sheet.scss';
    @import './action-sheet.ios.scss';
    @import './action-sheet.md.scss';
    @import './action-sheet.wp.scss';
    .ion-action-sheet-fade-enter-active {
        transition: transform .4s cubic-bezier(.36, .66, .04, 1);
        transform: translate3d(0, 0, 0);
    }
    
    .ion-action-sheet-fade-leave-active {
        transition: transform .3s cubic-bezier(.36, .66, .04, 1);
        transform: translate3d(0, 100%, 0);
    }
    
    .ion-action-sheet-fade-enter,
    .ion-action-sheet-fade-leave-to {
        transform: translate3d(0, 100%, 0);
    }
</style>