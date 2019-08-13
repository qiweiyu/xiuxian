<template>
    <table>
        <tr>
            <th colspan="2">
                <router-link :to="{name: 'view', query: {address: name}}">
                    <img src="@/assets/person.jpg"/>
                </router-link>
            </th>
        </tr>
        <tr>
            <td>道号</td>
            <td>{{ name }}</td>
        </tr>
        <tr>
            <td>修为</td>
            <td>{{ staking }}</td>
        </tr>
        <tr>
            <td>道行</td>
            <td>{{ balance }}</td>
        </tr>
        <tr>
            <td>突破修为概率</td>
            <td>23.333333%</td>
        </tr>
    </table>
</template>

<script>
    export default {
        name: 'Person',
        props: {
            name: String,
            balance: String,
            staking: Number,
        },
    }
</script>

<style scoped>
    table {
        border: solid 1px;
        width: 50px;
        height: 600px;
    }

    table td {
        border: solid 1px;
    }
</style>
