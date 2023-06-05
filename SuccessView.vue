<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Thank you for shopping!</h1>

                <p>You will get your order(s) delivered to your doorstep in 24 hours.</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SuccessView',
    mounted() {
        document.title = 'Success | MoshionApparels'
    },
}
</script>