<template>
    <div>
        <Person name="abc" balance="123"/>
    </div>
</template>

<script>
    // @ is an alias to /src
    import Person from '@/components/Person.vue'

    export default {
        name: 'home',
        components: {
            Person,
        },
    }
</script>
