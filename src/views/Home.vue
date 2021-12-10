<template>
  <div class="container mx-auto">
    <h3 class="text-center text-2xl mt-5">ToDo List</h3>
    <div class="flex align-items-center w-1/2 mx-auto mt-3">
      <input type="text" name="newGame" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" v-model="newGame" placeholder="Enter Game Name" />
      <input type="text" name="newAmount" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" v-model="newAmount" placeholder="Enter Game Amount" />
      <button @click="createGame" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button">Create</button>
    </div>
    <ul class="mt-5 mx-auto text-center">
      <li v-for="(g, i) in games" :key="i">
        {{g.name}} - {{g.amount}}
      </li>
    </ul>
  </div>
</template>

<script>
import Web3 from 'web3'
const contractABI = 
      [
        {
          "inputs": [
            {
              "internalType": "string",
              "name": "name",
              "type": "string"
            },
            {
              "internalType": "uint256",
              "name": "amount",
              "type": "uint256"
            }
          ],
          "name": "addGame",
          "outputs": [],
          "stateMutability": "nonpayable",
          "type": "function"
        },
        {
          "inputs": [],
          "name": "gameCount",
          "outputs": [
            {
              "internalType": "uint256",
              "name": "",
              "type": "uint256"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        },
        {
          "inputs": [
            {
              "internalType": "uint256",
              "name": "index",
              "type": "uint256"
            }
          ],
          "name": "getGame",
          "outputs": [
            {
              "internalType": "string",
              "name": "name",
              "type": "string"
            },
            {
              "internalType": "uint256",
              "name": "amount",
              "type": "uint256"
            }
          ],
          "stateMutability": "view",
          "type": "function"
        }
      ];
export default {
  data() {
    return {
      web3Provider: null,
      account: null,
      web3: null,
      contracts: {},
      loading: false,
      games: [],
      newGame: '',
      newAmount: '',
      id: 0
    }
  },
  created() {
    this.load();
  },
  methods: {
    async load() {
      await this.loadWeb3();
      await this.loadAccounts();
      await this.loadContract();
      await this.renderGames();
    },
    async loadWeb3() {
      // this.web3 = window.web3;
      // console.log('web3', this.web3);
      if(typeof window.web3 !== 'undefined') {
        this.web3Provider = window.web3.currentProvider;
        window.web3 = new Web3(window.web3.currentProvider);
      } else {
        window.alert("Please connect to metamask");
      }

      if(window.ethereum) {
        window.web3 = new Web3(window.ethereum);
        try {
          await window.ethereum.enable();
        } catch (error) {
          console.log('error',error);
        }
      } else if (window.web3) {
        this.web3Provider = window.web3.currentProvider;
        window.web3 = new Web3(window.web3.currentProvider);
      } else {
          console.log('Non-Ethereum browser detected. You should consider trying MetaMask!')
      }
    },
    async loadAccounts() {
      // this.account = window.web3.eth.accounts.create().address;
      // console.log('account',this.account);
      let accounts = await window.ethereum.request({
          method: 'eth_requestAccounts'
        });
        console.log('Account', accounts)
        this.account = accounts[0];
        console.log('0 Account', this.account)
    },
    async loadContract() {
      this.contract = await new window.web3.eth.Contract(
      contractABI, '0x5a02B326E52A0CAF154176B1275cCc1Fd42E3d07', {
          from: this.account
      });  
    },

    async renderGames() {
      // // Load the total task count from the blockchain
      const gameCount = await this.contract.methods.gameCount().call();
      console.log('gameCount',this.contract.methods);
      this.games = []
      for (var i = 0; i <= gameCount; i++) {
        // Fetch the task data from the blockchain
        const game = await this.contract.methods.getGame(i).call();
        console.log('game',game[0]);
        let gameJson = {
          name: game[0],
          amount: game[1]
        }
        this.games.push(gameJson)
      }
    },
    async createGame() {
        // this.loading = true
        const g = await this.contract.methods.addGame(this.newGame, this.newAmount).send({
          from: this.account
        });
        console.log('ggg',g);
        this.newGame = '';
        this.newAmount = '';
        // this.loading = false
        // this.newTaskCont = ''
        this.renderGames()
      },
  }

}
</script>