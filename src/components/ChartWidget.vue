<template>
  <div class="temp"></div>
</template>
<script>
export default {
	name: "ChartWidget",
	data() {
		return {};
	},
	props: {
		currency: {
			type: String,
			required: true
		}
	},
	created() {
		const socket = new WebSocket("ws://coins-stream.demo.javascript.ninja");
		socket.onopen = () => {
			const data = { type: "subscribe", currency: this.currency };
			socket.send(JSON.stringify(data));
		};

		socket.onclose = event => {
			if (event.wasClean) {
				// eslint-disable-next-line no-console
				console.log("Соединение закрыто чисто");
			} else {
				// eslint-disable-next-line no-console
				console.log("Обрыв соединения"); // например, "убит" процесс сервера
			}
			// eslint-disable-next-line no-console
			console.log(`Код: ${event.code} причина:  ${event.reason}`);
			// eslint-disable-next-line no-console
			console.log(event);
		};

		socket.onmessage = event => {
			// eslint-disable-next-line no-console
			console.log(`Получены данные ${event.data}`);
			// eslint-disable-next-line no-console
			console.log(event);
		};

		socket.onerror = error => {
			// eslint-disable-next-line no-console
			console.log(`Ошибка  ${error.message}`);
		};
	},
	computed: {},
	methods: {},
	beforeDestroy() {}
};
</script>
<style>

</style>
