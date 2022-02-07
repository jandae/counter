<template>
	<div id="app">		
		<div>Counters</div>
		<div class="counter">
			<div>Sketchy Moves</div>
			<Roller :text='nums["num0"]'></Roller>
		</div>
		<div class="counter">
			<div>Bad Stops</div>
			<Roller :text='nums["num1"]'></Roller>
		</div>
	</div>
</template>

<script>
import Roller from "vue-roller";

export default {
	name: 'App',
	components: {
		Roller
	},	
	data () {
		return {
			num: "00",			
			num1: "00",			
			nums: {
				num0: "00",
				num1: "00"
			}
		}
	},
	mounted () {		
		this.ws()
	},
	methods: {
		ws: function () {
			let $this = this			
			
			console.log("Connecting to monty WebSocket Server")			
			this.connection = new WebSocket('ws://localhost:1880/counter')

			setInterval(function(){
				if ($this.connection.readyState == 3) {
					console.log('reloading')
					location.reload()
				}
			}, 5000)
			
			this.connection.onmessage = function(event) {
				let msg_data = JSON.parse(event.data)
				console.log(msg_data)
				if(msg_data.op == 'add') {
					$this.nums["num"+msg_data.counter] = msg_data.val
					$this.add(msg_data.counter)
					console.log('+')
					console.log($this.nums)
				} else if(msg_data.op == 'sub') {
					$this.nums["num"+msg_data.counter] = msg_data.val
					$this.sub(msg_data.counter)
					console.log('-')
				} 
			}

			this.connection.onopen = function() {				
				console.log("Successfully connected to monty websocket server...")
			}
		},
		add: function(i) {
			let n = parseInt(this.nums["num"+i])		
			this.nums["num"+i] = String(n).padStart(2,'0')
			console.log(this.nums)
		},
		sub: function(i) {
			let n = parseInt(this.nums["num"+i])
			this.nums["num"+i] = String(n).padStart(2,'0')
		}
	}
}
</script>

<style lang="scss">
body {
	background: #000;
}
#app {
	font-family: amuroregular, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #fff;
	margin-top: 60px;
	font-size: 2em;
}

.counter {
	display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
	line-height: 11px;
    margin-top: 10px;

	.roller {}
}

@font-face {
    font-family: 'amuro_condensedregular';
    src: url('./assets/fonts/amurocondensed-merp-webfont.woff2') format('woff2'),
         url('./assets/fonts/amurocondensed-merp-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amuroitalic';
    src: url('./assets/fonts/amuroitalic-wwx6-webfont.woff2') format('woff2'),
         url('./assets/fonts/amuroitalic-wwx6-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amuroregular';
    src: url('./assets/fonts/amuro-82yn-webfont.woff2') format('woff2'),
         url('./assets/fonts/amuro-82yn-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amurobold_italic';
    src: url('./assets/fonts/amurobolditalic-rwab-webfont.woff2') format('woff2'),
         url('./assets/fonts/amurobolditalic-rwab-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amurobold';
    src: url('./assets/fonts/amurobold-ykg2-webfont.woff2') format('woff2'),
         url('./assets/fonts/amurobold-ykg2-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amuro_condensedbold_italic';
    src: url('./assets/fonts/amurocondensedbolditalic-2bgo-webfont.woff2') format('woff2'),
         url('./assets/fonts/amurocondensedbolditalic-2bgo-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amuro_condensedbold';
    src: url('./assets/fonts/amurocondensedbold-kp8x-webfont.woff2') format('woff2'),
         url('./assets/fonts/amurocondensedbold-kp8x-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}




@font-face {
    font-family: 'amuro_condenseditalic';
    src: url('./assets/fonts/amurocondenseditalic-vgj9-webfont.woff2') format('woff2'),
         url('./assets/fonts/amurocondenseditalic-vgj9-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;

}
</style>
