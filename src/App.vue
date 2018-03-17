<template>
  <div id="app">
    <img src="./assets/logo.png">
    <Autocomplete v-model='selected' :getItems='getItems'>
		<span slot='itemInput' slot-scope="props" class="autocomplete__input">{{props.value.name}}</span>
		<span slot='itemList' slot-scope="props" class="autocomplete__result-item" >{{props.item.id}} {{props.item.name}} {{props.item.price}}</span>
	</Autocomplete>
	<!-- <ChartWidget currency='BTC'></ChartWidget> -->
	
  </div>
</template>

<script>
import Autocomplete from "./components/Autocomplete";
import ChartWidget from "./components/ChartWidget";

export default {
	name: "App",
	components: {
		Autocomplete,
		ChartWidget
	},
	data() {
		return {
			isActive: true,
			selected: { name: "ETH", price: 60, id: 1 }
		};
	},
	methods: {
		getItems() {
			const prom = new Promise(resolve => {
				const resp = {
					items: [
						{ name: "ETH", price: 60, id: 1 },
						{ name: "ETC", price: 30, id: 2 },
						{ name: "BCH", price: 24, id: 3 },
						{ name: "LTC", price: 10, id: 4 },
						{ name: "ETC", price: 3, id: 5 },
						{ name: "BNB", price: 100, id: 6 },
						{ name: "XRP", price: 45, id: 7 },
						{ name: "NEO", price: 38, id: 8 },
						{ name: "TRX", price: 11, id: 9 },
						{ name: "DASH", price: 4, id: 10 },
						{ name: "XMR", price: 38, id: 11 }
					]
				};
				setTimeout(() => {
					resolve(resp);
				}, 300);
			});
			prom.abort = () => {};
			return prom;
		}
	}
};
</script>
<style src='./style/normalize.css'></style>
<style>
#app {
	font-family: "Avenir", Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
* {
	margin: 0;
	padding: 0;
	list-style-type: none;
	outline: none !important;
	text-decoration: none;
	box-sizing: border-box;
	max-height: 999999px;
	-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

h1,
h2,
h3,
h4,
h5,
h6 {
	margin: 0;
}

html,
* html body {
	height: 100%;
}

body {
	min-height: 100%;
	min-width: 300px;
	color: #000;
	font-size: 15px;
	font-weight: 400;
	background-color: #fff;
}
</style>



