<script setup>
definePageMeta({
    layout: "test",
})

const route = useRoute()

const { data, error } = await useAsyncData(() => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({
                page: {
                    title: "dynamic page",
                    content: `<h2>Page</h2><div>page path: ${route.params.slug}</div>`,
                },
            })
        }, 500)
    })
})

if (error.value) {
  throw createError({ statusCode: error.value.statusCode || 502, fatal: import.meta.client })
}

if (!data.value?.page) {
  throw createError({ statusCode: 404, fatal: import.meta.client })
}
</script>

<template>
    <div>
        <h1>{{ data.page.title}}</h1>
        <div v-html="data.page.content"></div>
    </div>
</template>
