<template>
    <div class="card-header">
        <h1>Correct: {{numCorrect}}/{{numTotal}}</h1>
    </div>
</template>

<script>
export default {
    name: "Header",
    props: ["numCorrect", "numTotal"]
}
</script>

<style scoped>
</style>