<template>
  <v-flex xs6>
    <v-card class="card--flex-toolbar">
      <v-toolbar card class="red darken-4">
        <v-toolbar-title class="white--text">My Wallet</v-toolbar-title>
      </v-toolbar>
      <v-list class="grey darken-1">
        <v-list-tile>
          <v-list-tile-title>
            Address
          </v-list-tile-title>
          <v-list-tile-content>
            {{ coinbase }}
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile>
          <v-list-tile-title>
            Eth Balance
          </v-list-tile-title>
          <v-list-tile-content>
            {{ balance }}
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile>
          <v-list-tile-title>
            Token Balance
          </v-list-tile-title>
          <v-list-tile-content>
            {{ tokens }}
          </v-list-tile-content>
        </v-list-tile>

      </v-list>

    </v-card>
  </v-flex>
</template>
<script>
import { CONTRACT } from '../contract'

export default {
  data () {
    return {
      coinbase: 0x00,
      balance: 0,
      tokens: 0
    }
  },

  mounted () {
    this.coinbase = CONTRACT._eth.coinbase

    this.getEtherBalance()
    this.getTokenBalance()
    console.log("test");

    CONTRACT.Transfer((err, res) => {
      if(err){
        console.log(err)
      }
      console.log("refresh");
      this.getTokenBalance()
    });

  },

  methods: {
    getEtherBalance () {
      CONTRACT._eth.getBalance(this.coinbase, (err, bal) => {
        if (!err) {
          this.balance = web3.fromWei(bal, 'ether').toNumber()
        }
        console.log(err)
      })
    },

    getTokenBalance () {
      CONTRACT.balanceOf(this.coinbase, (err, tkns) => {
        if (!err) {
          this.tokens = web3.fromWei(tkns, 'ether').toNumber()
        }
        console.log(err)
      })
    }
  }
}
</script>
