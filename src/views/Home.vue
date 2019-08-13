<template>
    <div>
        <h1>Qtum 修仙区块浏览器</h1>
        <ul>
            <li v-for="person in personList">
                <Person
                        :name="person.address"
                        :balance="person.balance"
                        :staking="person.blocks"
                ></Person>
            </li>
        </ul>
    </div>
</template>

<script>
    import Person from '@/components/Person.vue'
    import axios from 'axios'

    export default {
        components: {
            Person,
        },
        data() {
            return {
                personList: [],
            }
        },
        mounted() {
            axios.get('https://qtum.info/api/misc/biggest-miners?page=0&pageSize=100').then(res => {
                this.personList = res.data.list
            })
        },
    }
</script>

<style scoped>
    li {
        float: left;
    }
</style>
