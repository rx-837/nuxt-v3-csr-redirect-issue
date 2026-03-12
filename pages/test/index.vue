<script setup>
definePageMeta({
    layout: "test",
})

const event = useRequestEvent()
const route = useRoute()

const { data, error } = await useAsyncData(() => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({
                path: "/default-page"
            })
        }, 500)
    })
})

if (data.value?.path) {
    await navigateTo({
        path: `/test${data.value.path}`,
        query: route.query,
    })
} else {
  error.value = error.value || createError({ statusCode: 404 })
  if (event) {
    setResponseStatus(event, error.value.statusCode || 502)
  }
}

</script>

<template>
    <div v-if="error">
        errorCode: {{ error.statusCode }}
    </div>
</template>
