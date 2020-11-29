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
				<div class="chats" id="chats" style="overflow: auto">
					<div class="message-container">
						<img src="../../public/bird.png" alt="Avatar" class="ai avatar" />
						<p class="reply">Hi, I'm ChatBot! How can I help you today?</p>
					</div>
				</div>
				<div class="send-container">
					<textarea id="msg" placeholder="Type message..." name="msg" v-model="msg" required v-on:keyup.enter="sendUserMessage('textarea')"></textarea>
					<button class="send-button" id="send" v-on:click.prevent="sendUserMessage(msg)" type="button">Send</button>
				</div>
			</form>
		</div>
		<footer></footer>
	</div>
</template>

<script>
import { Component, Vue } from 'vue-property-decorator';
import Botlang from 'botlang';

export default {
	data() {
		return {
			msg: '',
			datesSelectedFlag: false,
			roomTypeSelectedFlag: false,
			bookingConfirmed: false
		};
	},
	methods: {
		openForm() {
			const form = document.querySelector('#myForm');
			form.style.display = 'block';
			const textArea = document.querySelector('#msg');
			textArea.focus();
		},
		closeForm() {
			const form = document.querySelector('#myForm');
			form.style.display = 'none';
		},
		sendUserMessage(msg) {
			const chat = document.getElementById('chats');
			//chat.scrollTop = chat.scrollHeight - chat.clientHeight;
			const textArea = document.querySelector('#msg');
			if (msg === 'textarea') {
				msg = textArea.value;
			}
			// Clear the text area
			textArea.value = '';

			// Send the message bubble into the chat
			const messageBubble = document.querySelector('.chats');
			if (messageBubble) {
				messageBubble.innerHTML += "<div class='message-container darker'>" + "<img src='/bird.png' alt='Avatar' class='user avatar' />" + "<p class='message'>" + msg + '</p></div>';
			}
			chat.lastChild.scrollIntoView();

			// Now parse the message to craft a response
			this.parseMessage(msg);
		},
		sendResponseMessage(msg) {
			const chat = document.getElementById('chats');

			//chat.scrollTop = chat.scrollHeight - chat.clientHeight;
			// Send the message bubble into the chat
			const messageBubble = document.querySelector('.chats');
			if (messageBubble) {
				messageBubble.innerHTML += "<div class='message-container'>" + "<img src='/bird.png' alt='Avatar' class='ai avatar' />" + "<p class='reply'>" + msg + '</p></div>';
			}
			chat.lastChild.scrollIntoView();
		},
		parseMessage(msg) {
			// Initialize the language bot
			const bot = this.initBot();
			let msgType = '';
			// Get the response and parse it out
			let botResponse = bot.reply(msg);
			if (msg.indexOf(':') > -1) {
				msgType = botResponse.split(':')[0];
			} else {
				msgType = '';
			}
			const dateAvailable = Math.floor(Math.random() * 10 + 1);
			const roomAvailable = Math.floor(Math.random() * 10 + 1);

			// Figure out what to do based on the msgType
			switch (msgType) {
				case 'dateConfirm':
					// Important!! Do a check or some kind of random function for if there are rooms available for those dates
					if (dateAvailable <= 5) {
						// If dates not available, prompt again
						this.sendResponseMessage(bot.reply('xx01'));
					} else {
						// If dates available, send message and set flag to true
						this.datesSelectedFlag = true;
						this.sendResponseMessage(bot.reply('xx02'));
					}
					break;
				case 'suiteConfirm':
					// Important!! Do a check or some kind of random function for which types of rooms available
					// can be all types or nothing, or a combo of which are available and which not
					if (roomAvailable <= 5 && this.datesSelectedFlag === true) {
						this.sendResponseMessage(bot.reply('xx03'));
					} else {
						this.roomTypeSelectedFlag = true;
						this.sendResponseMessage(bot.reply('xx04'));
					}
					break;
				case 'twoConfirm':
					// Important!! Do a check or some kind of random function for which types of rooms available
					// can be all types or nothing, or a combo of which are available and which not
					if (roomAvailable <= 5 && this.datesSelectedFlag === true) {
						this.sendResponseMessage(bot.reply('xx05'));
					} else {
						this.roomTypeSelectedFlag = true;
						this.sendResponseMessage(bot.reply('xx06'));
					}
					break;
				case 'oneConfirm':
					// Important!! Do a check or some kind of random function for which types of rooms available
					// can be all types or nothing, or a combo of which are available and which not
					if (roomAvailable <= 5 && this.datesSelectedFlag === true) {
						this.sendResponseMessage(bot.reply('xx07'));
					} else {
						this.roomTypeSelectedFlag = true;
						this.sendResponseMessage(bot.reply('xx08'));
					}
					break;
				case 'generalConfirm':
					// Check the flags for which confirm this applies to and return one of the appropriate next steps

					if (this.roomTypeSelectedFlag && this.roomTypeSelectedFlag) {
						// If date and room type have been confirmed, send a wrap up message and reset everything
						this.sendResponseMessage(bot.reply('xx11'));
					} else if (this.datesSelectedFlag === true && this.roomTypeSelectedFlag === false) {
						// If only date, ask for room type
						this.sendResponseMessage(bot.reply('xx12'));
					} else {
						// If none or only room type, ask for date
						this.sendResponseMessage(bot.reply('xx01'));
					}
					break;
				case 'cancel':
					// Return an appropriate goodbye message and reset (resetEverything function)
					//TODO add cancel into script
					this.resetEverything();
					this.sendResponseMessage(parsedMessage);
					break;
				default:
					// Return the parsed message the bot gave out
					this.sendResponseMessage(parsedMessage);
					break;
			}
		},
		resetEverything: function() {
			this.datesSelectedFlag = false;
			this.roomTypeSelectedFlag = false;
		},
		initBot: function() {
			// Using a string literal for the script as a last ditch resort
			// Book / Make - Room / Reservation > Get date > Get room > confirm
			// ... > Get date > Get new date > ...
			// ... > Get room > Get new room > ...
			// ... > no confirm > ...
			const scriptString =
				'# Botlang Script' +
				'\n+ "*"' +
				'\n- "I\'m not sure I understand you fully."' +
				'\n- "I\'m sorry, I don\'t understand."' +
				'\n- "Could you rephrase that? I\'m having trouble understanding."' +
				'\n\n+ "Hello?"' +
				'\n- "Hi! How can I help you?"' +
				'\n- "Hello!"' +
				'\n\n+ "How are you?"' +
				'\n- "I\'m doing well, thank you!"' +
				'\n- "I\'m doing well, how about you?"' +
				'\n- "I\'m doing good, how are you?"' +
				'\n- "I\'m fantastic! How are you?"' +
				'\n\n+ "Thank you!"' +
				'\n- "You\'re welcome!"' +
				'\n- "You\'re very welcome."' +
				'\n- "Of course! You\'re welcome!"' +
				'\n- "Anytime, You\'re welcome!"' +
				'\n\n+ "Help"' +
				'\n- "Alright, tell me how I can help!"' +
				'\n- "Please let me know how I can help!"' +
				'\n- "What can I help you with?"' +
				'\n\n+ "* (book|reservation|room) *"' +
				'\n- "Do you have a date range?"' +
				'\n- "And what dates are you looking to book?"' +
				'\n- "Okay! When would your stay be?"' +
				'\n- "Can I get the dates You\'re looking to book?"' +
				'\n- "What are the beginning and end dates of your stay?"' +
				'\n- "Can do! What dates are you looking to book?"' +
				'\n\n+ "* (book|make) reservation"' +
				'\n- "Can I get the dates of the reservation?"' +
				'\n- "And what are the dates you are looking to reserve?"' +
				'\n- "Okay! Can I get the dates of the reservation you\'d like to make?"' +
				'\n- "Awesome! When are you thinking about booking?"' +
				'\n- "Ok what dates are you looking to book?"' +
				'\n\n+ "* to *"' +
				'\n- "dateConfirm: DATE CHECK"' +
				'\n\n+ "xx01"' +
				'\n- "I\'m sorry, there are no available rooms for those dates."' +
				'\n- "I\'m sorry, we\'re all booked those dates."' +
				'\n- "There are no rooms available for those dates, are there any other datest that will work for you?"' +
				'\n- "There are no rooms available for those dates, I\'m sorry!"' +
				'\n\n+ "xx02"' +
				'\n- "We have some rooms available, what type were you looking to book?"' +
				'\n\n+ "* (suite) *"' +
				'\n- "suiteConfirm: SUITE CHECK."' +
				'\n\n+ "xx03"' +
				'\n- "I\'m sorry, our suite is already booked for those dates."' +
				'\n- "Our suite is not available for those dates, is there another date range you\'d be able to book?"' +
				'\n- "Sorry our suite is not available for those dates, is there another room you\'d like to book for those dates?"' +
				'\n+ "xx04"' +
				'\n- "Good news! Our suite is available for those dates! It\'s 100€ a night and has a lovely view."' +
				'\n- "Alright, our suite is available for those dates, lets get you booked! It\'s 100€ a night, is that alright?"' +
				'\n- "The suite it is! I just want to confirm with you that It\'s 100€ a night."' +
				'\n- "Can do, you\'ll be booked for those dates in the suite, then, at 100€ a night!"' +
				'\n+ "* (two bed) *"' +
				'\n- "twoConfirm: TWO BED CHECK"' +
				'\n\n+ "xx05"' +
				'\n- "Good news! We have a two bed room available for those dates! It\'s 50€ a night and quite comfortable."' +
				'\n- "Alright, there are two bed rooms available for those dates, lets get you booked! It\'s 50€ a night, is that alright?"' +
				'\n- "Can do, you\'ll be booked for those dates in a two bed, then, at 100€ a night!"' +
				'\n\n# if two bed is not available' +
				'\n+ "xx06"' +
				'\n- "Sorry, our two bed rooms are all booked up for those dates."' +
				'\n- "I\'m sorry, all of our two bed rooms are already booked for those dates. Are there any other rooms or dates that work for you?"' +
				'\n+ "* (one bed) *"' +
				'\n- "oneConfirm: ONE BED CHECK"' +
				'\n\n+ "xx07"' +
				'\n- "Good news! We have a one bed room available for those dates! It\'s 25€ a night and quite comfortable."' +
				'\n- "Alright, there are one bed rooms available for those dates, lets get you booked! It\'s 25€ a night, is that alright?"' +
				'\n- "Can do, you\'ll be booked for those dates in a two bed, then, at 25€ a night!"' +
				'\n+ "xx08"' +
				'\n- "Sorry, our one bed rooms are all booked up for those dates."' +
				'\n- "I\'m sorry, all of our one bed rooms are already booked for those dates. Are there any other rooms or dates that work for you?"' +
				'\n\n+ "(goodbye|bye|see ya)"' +
				'\n- "cancel: Goodbye. Come back soon!"' +
				'\n- "cancel: Bye now. Have a nice day!"' +
				'\n- "cancel: Thank you for visiting. Hope we see you soon!"' +
				'\n\n+ "* (change|switch|swap) room"' +
				'\n- "What type of room would you like instead?"' +
				'\n- "Alright, what type of room would you rather have?"' +
				'\n\n+ "* (change|reschedule) the (date|day)"' +
				'\n- "What days work better for you?"' +
				'\n- "When would you like to reschedule your reservation?"';

			// Init and return the bot with the script
			const bot = new Botlang(scriptString);
			return bot;
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
// MESSAGE CSS - make these able to stretch if longer message?
/* Chat containers */
.message-container {
	border: 2px solid #dedede;
	border-radius: 5px;
	padding: 10px;
	margin: 10px 0;
	min-height: 2.5em;
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
