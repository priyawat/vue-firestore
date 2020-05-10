<template>
	<div id="app">
		<img width=200 src="https://d33wubrfki0l68.cloudfront.net/ca0061c3c33c88b2b124e64ad341e15e2a17af49/c8765/images/alligator-logo3.svg">
		<div>
			<input type="text" v-model="newReptile" @keyup.enter="addReptile">
			<button  @click="addReptile">Add Reptile</button>
		</div>
		<ul class="reptileList">
			<li v-for="reptile in reptiles" >
				{{ reptile.name }} -
				<button @click="deleteReptile(reptile)">Remove</button>
			</li>
		</ul>
	</div>

</template>

<script>
	import { db } from './firebase';

	export default {
    name: 'app',
		data() {
			return {
				reptiles: [],
				newReptile: ''
			}
		},
		firestore() {
			return {
				reptiles: db.collection('reptiles'),
			}
		},
		methods: {
			addReptile: function() {
        this.$firestore.reptiles.add(
					{
            name: this.newReptile,
            timestamp: new Date
        	}
				)
				this.newReptile = '';
    	},
			deleteReptile: function(reptile) {
				this.$firestore.reptiles.doc(reptile['.key']).delete()
			}	
		}
  }
</script>

<style>
	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
	.reptileList {
		list-style: none;
	}
</style>