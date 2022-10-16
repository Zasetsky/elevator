<template>
    <div>
        <button 
            class="floorButton" 
            @click="buttonClick()" 
            :class="{active: floorsButton[id].isActive}" 
        >◎</button>
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
        width: 30px;
        height: 25px;
        margin: 10px 0 32px 10px;
        background: #76e6ff;
    }
    .active {
        background: #fff876;
    }

    .fiveButtons {
        margin: 25px 0 53px 10px;
    }
</style>