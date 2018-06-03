<template>
    <div class="modal" :class="{ 'modal--visible': visible }">
        <div class="modal-body">
            <div class="modal-title">{{ title }}</div>
            <div class="modal-content">
                <slot></slot>
            </div>
            <div class="modal-buttons">
                <a class="btn btn--link" @click="$emit('cancel');">Cancel</a>
                <a class="btn" @click="$emit('action');">OK</a>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Modal',
        props: {
            title: null,
            content: null,
            visible: false
        }
    }
</script>

<style scoped>
    .modal {
        position        : fixed;
        top             : 0;
        left            : 0;
        right           : 0;
        bottom          : 0;
        z-index         : 100;
        opacity         : 0;
        visibility      : hidden;
        display         : flex;
        align-items     : center;
        justify-content : center;
        background      : rgba(0, 0, 0, 0.5);
    }

    .modal--visible {
        opacity    : 1;
        visibility : visible;
    }

    .modal-body {
        background          : #fff;
        border-radius       : 20px;
        overflow            : hidden;
        color               : #000;
        width               : 520px;
        max-width           : 98%;
        box-shadow          : 0 5px 10px rgba(0, 0, 0, 0.2);
        transform           : scale(1.2);
        transition-duration : .3s;
    }

    .modal--visible > .modal-body {
        transform : none;
    }

    .modal-title {
        font-size      : 1.4rem;
        font-weight    : bold;
        text-align     : center;
        text-transform : uppercase;
        padding        : 20px 10px;
        background     : #f6f6f6;
    }

    .modal-content {
        padding   : 30px;
        font-size : 1rem;
    }

    .modal-buttons {
        border-top : 1px solid #eee;
        text-align : right;
        padding    : 12px 15px;
    }
</style>