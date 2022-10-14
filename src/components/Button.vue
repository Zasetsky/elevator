<template>
    <div>
        <button class="floor__button" @click="buttonClick()" :class="{active: floorsButton[id].isActive}" />
    </div>
</template>
<script>
export default {
    name: 'ButtonComponent',
    methods: {
        buttonClick() {
            this.$emit('buttonClick', this.floorsButton[this.id].id)
        },
    }
}
</script>
<style lang="">
    
</style>