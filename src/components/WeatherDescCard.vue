<template>
    <div>
        <div class="name">
            <slot name="name"></slot>
        </div>
        <div class="desc">
            <slot name="desc"></slot>
        </div>
    </div>
</template>

<style scoped>
.name {
    color: gray;
    font-size: 0.6rem;
}
</style>