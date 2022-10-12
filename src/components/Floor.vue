<template>
    <div class="floor">
        <button id="button" @click="buttonClick()" :class="{active: isActive}" />
    </div>
</template>
<script>
export default {
    name: 'FloorComponent',
    props: ['id'],
    data() {
        return {
            isActive: false
        }
    },
    methods: {
        buttonClick() {
            this.isActive = !this.isActive
            this.$emit('buttonClick', this.id)
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
        button {
            padding: 10px;
            margin-left: 120px;
            margin-top: 30px;
            .active {
                padding: 100;
            }
        }
    }
    
</style>