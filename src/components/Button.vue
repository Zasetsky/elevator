<template>
    <div>
        <button class="floorButton" @click="buttonClick()" :class="{active: floorsButton[id].isActive}" />
    </div>
</template>
<script>
export default {
    name: 'ButtonComponent',
    props: ['floorsButton', 'id'],
    methods: {
        buttonClick() {
            this.$emit('buttonClick', this.floorsButton[this.id].id)
        },
    }
}
</script>
<style>
    .floorButton {
        padding: 10px;
        margin: 35px 0 65px 20px;

    }
</style>