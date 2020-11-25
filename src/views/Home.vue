<template>
	<div class="home">
		<h1>The Bird Bath</h1>
		<h2>Hang with the birds</h2>
		<p>
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus lacus sem, eget semper tortor pellentesque non. In vitae volutpat neque. Etiam condimentum laoreet lacus non laoreet. Etiam
			sagittis gravida lacus in imperdiet. Ut nulla nisi, imperdiet et lobortis ut, vestibulum et tortor. Integer ac euismod nulla. Suspendisse posuere diam eget erat sagittis, ut tincidunt mi
			ultrices. Vestibulum ut tortor nibh. Nam ultricies orci vitae lobortis sagittis.
		</p>
		<img class="bird-hotel" src="../..//public/hotel.jpg" />
		<p>
			Proin facilisis nisl eu lacus maximus, eu facilisis leo aliquet. Cras mollis laoreet diam, nec euismod enim facilisis in. Nullam feugiat gravida arcu quis dignissim. Vivamus sed dapibus lorem.
			Curabitur nec dolor non quam consectetur sagittis ac sed ex. Donec vitae orci enim. In ut vulputate risus. Integer pretium eros turpis, sit amet malesuada dolor tincidunt at. Nullam porta lacus
			et nibh venenatis, vel consequat metus porttitor. Nulla facilisi. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc libero lacus, condimentum nec lacinia a, blandit ut lorem.
		</p>
		<div class="image-container">
			<img class="hotel-room-images" src="../..//public/room1.jpg" />
			<img class="hotel-room-images" src="../..//public/room2.jpg" />
			<img class="hotel-room-images" src="../..//public/bath.jpg" />
		</div>
		<button class="book-now">BOOK NOW</button>

		<button class="open-button" @click="openForm()"><i class="material-icons">&#xe0b7;</i></button>

		<div class="chat-popup" id="myForm">
			<form class="form-container">
				<button type="button" class="cancel-button" @click="closeForm()">X</button>
				<h3 class="chat-title">Customer Service Chat</h3>
				<div class="chats" style="overflow: auto">
					<div class="message-container">
						<img src="../../public/bird.png" alt="Avatar" class="ai avatar" />
						<p class="reply">Hello. How are you today?</p>
					</div>

					<div class="message-container darker">
						<img src="../../public/bird.png" alt="Avatar" class="user avatar" />
						<p class="message">Hey! I'm fine. Thanks for asking!</p>
					</div>
				</div>
				<!-- TODO when message is sent, create new element and add child into message div -->
				<div class="send-container">
					<textarea id="msg" placeholder="Type message..." name="msg" v-model="msg" required></textarea>
					<!-- TODO change message button to be handled by 'Enter' -->
					<button class="send-button" v-on:click.prevent="sendUserMessage(msg)" type="submit">Send</button>
				</div>
			</form>
		</div>
		<footer></footer>
	</div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

export default {
	data(){
		return{
			msg:''
		}
	},
	methods: {
		openForm() {
			const form = document.querySelector('#myForm') as HTMLElement;
			form.style.display = 'block';
		},
		closeForm() {
			const form = document.querySelector('#myForm') as HTMLElement;
			form.style.display = 'none';
		},
		sendUserMessage: function(msg:string){
			// Clear the text area
			const textArea = document.querySelector('#msg') as HTMLInputElement;
			textArea.value = "";

			// Send the message bubble into the chat
			const messageBubble = document.querySelector('.chats') as HTMLElement;
			if(messageBubble){
				messageBubble.innerHTML += "<div class='message-container darker'>"
					+"<img src='/bird.png' alt='Avatar' class='user avatar' />"
					+"<p class='message'>"+msg+"</p></div>";
			}

			// Now parse the message to craft a response
			this.parseMessage(msg)
		},
		sendResponseMessage: function(msg:string){
			// Send the message bubble into the chat
			const messageBubble = document.querySelector('.chats') as HTMLElement;
			if(messageBubble){
				messageBubble.innerHTML += "<div class='message-container'>"
						+ "<img src='/bird.png' alt='Avatar' class='ai avatar' />"
						+ "<p class='reply'>" + msg + "</p></div>"
			}
		},
		parseMessage:function (msg:string){
			// Work in progress
			// console.log("Here I am")
			// const sourceCode = fs.readFileSync('C:\\Users\\veron\\OneDrive\\Documents\\WMU\\04 Senior Year\\hotel-help-chat\\src\\botlang-script.txt', {
			// 	encoding : 'utf8',
			// 	flag     : 'r'
			// }),
			// bot = new Botlang(sourceCode);

			// console.log(
			// 	bot.reply('user-input')
			// );

			this.sendResponseMessage(msg);
		}
	}

};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');
h1 {
	padding-top: 1em;
	font-size: 72pt;
	color: #0b2027;
	font-family: 'Dancing Script', cursive;
	margin: 0px;
}
h2 {
	font-size: 48pt;
	color: #0b2027;
	font-family: 'Dancing Script', cursive;
	margin: 0 0 1em 0;
}
p {
	margin: 0 2em 3em 2em;
	font-size: 16pt;
}
.bird-hotel {
	margin: auto auto;
	border: 10px solid #957d95;
	margin-bottom: 2em;
}
.book-now {
	font-size: 18pt;
	background-color: #40798c;
	color: white;
	font-family: 'Open Sans', sans-serif;
	border: none;
	margin: 2em auto 0 auto;
	&::hover {
		background-color: #957d95;
	}
}
.home {
	background-image: url('../../public/foggy_birds.png');
}
.hotel-room-images {
	width: 400px;
	height: 400px;
	border: 10px solid #957d95;
}
.image-container {
	display: flex;
	justify-content: space-between;
	margin: 0 40px 0 40px;
}
// CHAT BOX CSS
/* Button used to open the chat form - fixed at the bottom of the page */
.open-button {
	background-color: #40798c;
	color: white;
	padding: 16px 20px;
	border: none;
	border-radius: 10px;
	cursor: pointer;
	position: fixed;
	bottom: 20px;
	right: 40px;
	width: 80px;
}
/* The popup chat - hidden by default */
.chat-popup {
	display: none;
	position: fixed;
	bottom: 0;
	right: 15px;
	border: 3px solid #f1f1f1;
	z-index: 9;
}
/* Add styles to the form container */
.form-container {
	max-width: 450px;
	padding: 10px;
	background-color: white;
}
.chats {
	height: 350px;
}
.send-container {
	display: flex;
	align-items: center;
}
/* Full-width textarea */
.form-container textarea {
	width: 300px;
	padding: 10px;
	margin-right: 10px;
	border: none;
	background: #f1f1f1;
	resize: none;
	display: inline;
	font-size: 12pt;
}
/* When the textarea gets focus, do something */
.form-container textarea:focus {
	background-color: #ddd;
	outline: none;
}
.cancel-button {
	background-color: white;
	position: absolute;
	display: inline;
	right: 5px;
	top: 5px;
	border: none;
	outline: none;
	font-size: 16pt;
	cursor: pointer;
}
.send-button {
	background-color: #4caf50;
	color: white;
	padding: 10px;
	border: none;
	text-align: center;
	cursor: pointer;
	width: 60px;
	opacity: 0.8;
	display: inline;
}
.chat-title {
	display: inline;
}
// MESSAGE CSS
/* Chat containers */
.message-container {
	border: 2px solid #dedede;
	border-radius: 5px;
	padding: 10px;
	margin: 10px 0;
	height: 35px;
}
.darker {
	border-color: #957d959a;
}

.message {
	margin: 0px;
	padding: 0px;
	font-size: 12pt;
	width: 85%;
	margin-right: 45px;
	text-align: right;
}
.reply {
	margin: 0px;
	padding: 0px;
	font-size: 12pt;
	width: 85%;
	margin-left: 45px;
	text-align: left;
}

/* Clear floats */
.container::after {
	content: '';
	clear: both;
	display: table;
}

/* Style images */
.container img {
	float: left;
	max-width: 60px;
	width: 100%;
	margin-right: 20px;
	border-radius: 50%;
}

.ai {
	float: left;
	margin-left: 0px;
	margin-right: 20px;
}
.user {
	float: right;
	margin-left: 20px;
	margin-right: 0;
	transform: scaleX(-1);
}
.avatar {
	padding: 5px;
	width: 25px;
	border: 2px solid #40798c;
	border-radius: 30px;
}
</style>
