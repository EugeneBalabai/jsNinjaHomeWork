<template>
    <div class="autocomplete" ref='autocomplete'>
		<div class="autocomplete__input-wr">
			<slot name="itemInput" :value='value' :showDropdown='showDropdown'></slot>
			<input type="text" class="autocomplete__input transparent" v-model="inputVal" @focus="focusInput" @input='sendVal'>
		</div>
		
        <ul class="autocomplete__result-list" v-show="showDropdown">
			<li v-for="item in listData" :key="item.id" @click="selectItem(item)">
				<slot name="itemList" :item="item" ></slot>
			</li>
        </ul>
    </div>
</template>
<script>
export default {
	name: "Autocomplete",
	data() {
		return {
			showDropdown: false,
			inputVal: "",
			listData: [],
			savedPromise: {}
		};
	},
	props: {
		value: Object,
		getItems: Function
	},
	computed: {},
	mounted() {
		document.addEventListener("click", this.outsideClick);
	},
	beforeDestroy() {
		document.removeEventListener("click", this.outsideClick);
	},
	methods: {
		sendVal(e) {
			if (this.savedPromise && this.savedPromise.abort) {
				this.savedPromise.abort();
			}
			this.savedPromise = this.getItems(e);
			this.savedPromise.then(r => {
				this.listData = r.items;
				this.savedPromise = {};
			});
		},
		outsideClick(e) {
			if (!this.$refs.autocomplete.contains(e.target) && this.showDropdown) {
				this.selectItem({});
			}
		},
		focusInput() {
			this.inputVal = "";
			this.$emit("input", {});
			this.openDropdown();
		},
		openDropdown() {
			if (!this.showDropdown) {
				this.showDropdown = true;
			}
		},
		closeDropdown() {
			if (this.showDropdown) {
				this.showDropdown = false;
			}
		},
		selectItem(item) {
			this.inputVal = "";
			this.closeDropdown();
			this.$emit("input", item);
		}
	}
};
</script>
<style>
.autocomplete {
	margin: 20px auto 0;
	position: relative;
	width: 200px;
}
.autocomplete__input-wr {
	position: relative;
	width: 100%;
	height: 20px;
}
.autocomplete__input {
	position: absolute;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	border: 1px solid silver;
	width: 100%;
	outline: none;
	margin: 0;
	background: #fff;
	text-align-last: left;
}
.autocomplete__input.transparent {
	background: transparent;
}
.autocomplete__result-list {
	border: 1px solid silver;
	border-top: 0;
	position: absolute;
	left: 0;
	right: 0;
	top: 100%;
	overflow: auto;
	max-height: 93px;
	background: #fff;
}
.autocomplete__result-item {
	display: block;
	text-align: left;
}
.autocomplete__result-item:nth-child(even) {
	background: #f6f6f6;
}
.autocomplete__result-item:hover {
	background: #e5e5e5;
}
</style>

