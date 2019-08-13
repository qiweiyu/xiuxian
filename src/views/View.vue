<template>
    <div v-if="info">
        <h1>道友 {{ info.address }} 的修为情况</h1>
        <div>
            <div style="margin: 0 auto;display: table;">
                <Person
                        :name="info.address"
                        :balance="info.balance"
                        :staking="info.blocksMined"
                >
                </Person>
            </div>
        </div>
        <div v-if="txs">
            <h2 style="text-align: left;">最近道行动态</h2>
            <ul>
                <li v-for="tx in txs">
                    <template v-if="tx.isInput">
                        向道友
                        <template v-for="output in tx.outputs">
                            <router-link :to="{name: 'view', query: {address: output}}">{{output}}
                            </router-link>
                            &nbsp;
                        </template>
                        传送了
                        {{ tx.inputBalance }}
                        道行
                    </template>
                    <template v-else>
                        道友
                        <template v-for="input in tx.inputs">
                            <router-link :to="{name: 'view', query: {address: input}}">{{input}}
                            </router-link>
                            &nbsp;
                        </template>
                        传送了
                        {{ tx.outputBalance }}
                        道行
                    </template>
                </li>
            </ul>
        </div>
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
                info: null,
                txs: null,
            }
        },
        mounted() {
            const address = this.$route.query.address
            axios.get(`https://qtum.info/api/address/${address}`).then(res => {
                this.info = res.data
                this.info.address = address
            })
            axios.get(`https://qtum.info/api/address/${address}/txs?pageSize=10&page=0`).then(res => {
                const txIds = res.data.transactions
                if (txIds) {
                    const queries = []
                    txIds.forEach(txId => {
                        queries.push(axios.get(`https://qtum.info/api/tx/${txId}`))
                    })
                    Promise.all(queries).then(res => {
                        const txs = []
                        res.forEach(r => {
                            const inputs = {}
                            const outputs = {}
                            let inputBalance = 0
                            let outputBalance = 0
                            let isInput = false
                            let isOutput = false
                            r.data.inputs.forEach(input => {
                                inputs[input.address] = true
                                if (input.address === address) {
                                    isInput = true
                                    inputBalance += Number(input.value)
                                }
                            })
                            r.data.outputs.forEach(output => {
                                if (output.address) {
                                    outputs[output.address] = true
                                    if (output.address === address) {
                                        isOutput = true
                                        outputBalance += Number(output.value)
                                    }
                                }
                            })


                            txs.push({
                                data: r.data,
                                inputs: Object.keys(inputs),
                                outputs: Object.keys(outputs),
                                inputBalance,
                                outputBalance,
                                isInput,
                                isOutput,
                            })
                        })
                        this.txs = txs
                    })
                }
            })
        },
        watch: {
            '$route' (to, from) {
                this.$router.go(0);
            }
        },
    }
</script>

<style scoped>
    li {
        text-align: left;
    }
</style>