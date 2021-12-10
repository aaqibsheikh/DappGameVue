<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script>
// import Web3 from 'web3'
// const contractABI = 
//       [
//         {
//           "inputs": [
//             {
//               "internalType": "string",
//               "name": "name",
//               "type": "string"
//             },
//             {
//               "internalType": "uint256",
//               "name": "amount",
//               "type": "uint256"
//             }
//           ],
//           "name": "addGame",
//           "outputs": [],
//           "stateMutability": "nonpayable",
//           "type": "function"
//         },
//         {
//           "inputs": [],
//           "name": "gameCount",
//           "outputs": [
//             {
//               "internalType": "uint256",
//               "name": "",
//               "type": "uint256"
//             }
//           ],
//           "stateMutability": "view",
//           "type": "function"
//         },
//         {
//           "inputs": [
//             {
//               "internalType": "uint256",
//               "name": "index",
//               "type": "uint256"
//             }
//           ],
//           "name": "getGame",
//           "outputs": [
//             {
//               "internalType": "string",
//               "name": "name",
//               "type": "string"
//             },
//             {
//               "internalType": "uint256",
//               "name": "amount",
//               "type": "uint256"
//             }
//           ],
//           "stateMutability": "view",
//           "type": "function"
//         }
//       ];
// export default {
//   data() {
//     return {
//       web3Provider: null,
//       account: null,
//       web3: null,
//       contracts: {},
//       loading: false,
//       games: [],
//       newGame: '',
//       id: 0
//     }
//   },
//   created() {
//     this.load();
//   },
//   methods: {
//     async load() {
//       await this.loadWeb3();
//       await this.loadAccounts();
//       await this.loadContract();
//       await this.renderGames();
//     },
//     async loadWeb3() {
//       // this.web3 = window.web3;
//       // console.log('web3', this.web3);
//       if(typeof window.web3 !== 'undefined') {
//         this.web3Provider = window.web3.currentProvider;
//         window.web3 = new Web3(window.web3.currentProvider);
//       } else {
//         window.alert("Please connect to metamask");
//       }

//       if(window.ethereum) {
//         window.web3 = new Web3(window.ethereum);
//         try {
//           await window.ethereum.enable();
//         } catch (error) {
//           console.log('error',error);
//         }
//       } else if (window.web3) {
//         this.web3Provider = window.web3.currentProvider;
//         window.web3 = new Web3(window.web3.currentProvider);
//       } else {
//           console.log('Non-Ethereum browser detected. You should consider trying MetaMask!')
//       }
//     },
//     async loadAccounts() {
//       // this.account = window.web3.eth.accounts.create().address;
//       // console.log('account',this.account);
//       let accounts = await window.ethereum.request({
//           method: 'eth_requestAccounts'
//         });
//         console.log('Account', accounts)
//         this.account = accounts[0];
//         console.log('0 Account', this.account)
//     },
//     async loadContract() {
//       this.contract = await new window.web3.eth.Contract(
//       contractABI, '0x5a02B326E52A0CAF154176B1275cCc1Fd42E3d07', {
//           from: this.account
//       });  
//     },

//     async renderGames() {
//       // // Load the total task count from the blockchain
//       const gameCount = await this.contract.methods.gameCount().call();
//       console.log('gameCount',gameCount);
//       this.games = []
//       for (var i = 1; i <= gameCount; i++) {
//         // Fetch the task data from the blockchain
//         const game = await this.contract.methods.getGame(i).call();
//         console.log('game',game);
//         // let gameJson = {
//         //   id: game[0].toNumber(),
//         //   content: game[1],
//         //   complete: game[2]
//         // }
//         // console.log('Task', taskJson)
//         // parent.tasks.push(taskJson)
//       }
//         await this.createGame();
//     },
//     async createGame() {
//         // this.loading = true
//         const g = await this.contract.methods.addGame('Game3', 300).send({
//           from: this.account
//         });
//         console.log('ggg',g);
//         // this.loading = false
//         // this.newTaskCont = ''
//         // this.renderTasks(this)
//       },
//   }

// }
</script>