<template>
  <v-flex xs12>
    <v-card class="card--flex-toolbar">
      <v-toolbar card class="red darken-4">
        <v-toolbar-title class="white--text">Transactions</v-toolbar-title>
      </v-toolbar>

      <v-data-table :headers="headers" :items="items" hide-actions class="elevation-1">
        <template slot="items" scope="props">
          <td class="text-xs-left">{{ props.item.to }}</td>
          <td class="text-xs-left">{{ props.item.from }}</td>
          <td class="text-xs-left">{{ props.item.amount }}</td>
        </template>
      </v-data-table>

    </v-card>
  </v-flex>
</template>
<script>
import { CONTRACT } from '../contract'

export default {
  data () {
    return {
      headers: [
        { align: 'left', text: 'To', value: 'to' },
        { align: 'left', text: 'From', value: 'from' },
        { align: 'left', text: 'Amount', value: 'amount' }
      ],
      items: []
    }
  },

  mounted () {

    CONTRACT.Transfer({}, { fromBlock: 0, toBlock: 'pending' }, (err, res) => {
      console.log(res)
      if (res.args.to === CONTRACT._eth.coinbase || res.args.from === CONTRACT._eth.coinbase) {
        console.log("pushing to transaction history")
        this.items.push({
          to: res.args.to,
          from: res.args.from,
          amount: web3.fromWei(res.args.value).toNumber()
        })
      }
    })
  }
}
</script>
