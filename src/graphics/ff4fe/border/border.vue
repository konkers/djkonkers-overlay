<template>
<div>
    <div v-bind:style="contentDivStyle"><slot></slot></div>
    <div class="border" v-bind:style="borderDivStyle"></div>
</div>
</template>

<script lang="ts">
export default {
    props: ['window'],
    data() {
        const border_offset = 2;
        const r = this.window.rect;
        let borderDivStyle = { 
            left: `${r.p1.x - border_offset}px`,
            top: `${r.p1.y - border_offset}px`,
            width: `${r.p2.x - r.p1.x - border_offset * 2}px`,
            height: `${r.p2.y - r.p1.y - border_offset * 2}px`,
        };
        let contentDivStyle = { 
            position: "absolute",
            left: `${r.p1.x}px`,
            top: `${r.p1.y}px`,
            width: `${r.p2.x - r.p1.x}px`,
            height: `${r.p2.y - r.p1.y}px`,
        };
        if (this.window.cutout == false) {
            contentDivStyle['backgroundColor'] = '#000000';
        }
        return {
           borderDivStyle: borderDivStyle,
           contentDivStyle: contentDivStyle,
        }
    }
}
</script>

<style lang="scss">
    .border {
        position: absolute;
        border-style: solid;
        border-width: 4px;
        border-image: url('./9patch.png') 4 4 repeat
    }

</style> 