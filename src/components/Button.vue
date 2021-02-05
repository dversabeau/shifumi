<template>
    <div class="button-choice" @click="emitValue()">
        <div class="image-container">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Button",
        data: function () {
            return {
            }
        },
        props: {
            value: String,
            background: String,
        },
        methods: {
            emitValue: function () {
                this.$parent.$emit('PlayerValue', this.value)
            }
        }
    }
</script>

<style>

.button-choice{
    display: flex;
    align-items: center;
    justify-content: center;
    background: white;
    box-shadow: rgba(0,0,0,0.25) 0 4px inset;
    width: calc(100% - 30px);
    height: calc(100% - 30px);
    border-radius: 50%;
    cursor: pointer;
}

.image-container{
    width: 30px;
    height: auto;
    display: flex;
    justify-content: center;
    align-items: center;
}

.image-container>img{
    width: 100%;
    height: auto;
}

@media only screen and (min-width: 600px ) {

    .image-container{
        width: 40px;
    }

}

@media only screen and (min-width: 1024px ) {

    .button-choice{
        width: calc(100% - 50px);
        height: calc(100% - 50px);

    }

    .image-container{
        width: 60px;
    }

}

</style>