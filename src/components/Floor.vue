<template>
    <div class="floor">
        <button class="button" @click="buttonClick()" />
    </div>
</template>
<script>
export default {
    name: 'FloorComponent',
    props: ['id'],
    methods: {
        buttonClick() {
            this.$emit('buttonClick', this.id)
        }
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
        button {
            padding: 10px;
            margin-left: 120px;
            margin-top: 30px;
    }
    }
    
</style>