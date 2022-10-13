<template>
    <div class="floor">
        <button class="floor__button" @click="buttonClick()" :class="{active: floors.isActive}" />
    </div>
</template>
<script>
export default {
    name: 'FloorComponent',
    props: ['floorsButton', 'id'],
    methods: {
        buttonClick() {
            this.$emit('buttonClick', this.floorsButton[this.id].id)
        },
    }
}
</script>
<style lang="scss">
    .floor {
        border-left: solid 6px;
        border-right: solid 6px;
        border-color: black;
        margin-top: 25px;
        height: 100px;
        width: 99px;
        margin-left: 45px;
        &__button {
            padding: 10px;
            margin-left: 120px;
            margin-top: 30px;
            .active {
                padding: 100;
            }
        }
    }
    
</style>