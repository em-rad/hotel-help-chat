<template>
	<div class="home">
		<h1>The Bird Bath</h1>
		<h2>Hang with the birds</h2>
		<p class="covid">
			To help protect the health and safety of guests, employees, and visitors, we require the use of face coverings in all hotel public areas.<br />
			COVID-19 updates: Learn about our <span style="text-decoration: underline">booking policy</span> and health and <span style="text-decoration: underline">safety protocols</span>
		</p>
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
			const botResponse = bot.reply(msg);
			if (botResponse.indexOf(':') > -1) {
				msgType = botResponse.split(':')[0];
				msg = botResponse.split(':')[1];
			} else {
				msgType = '';
			}

			// YES / NO CHECK
			const confirmFormat = ['yes', 'no', 'ok', 'okay', 'confirm', 'that works', 'yep', 'sure', 'sounds good', 'yeah', 'no thank you', 'no thanks', 'nope', 'nevermind'];
			const msgCheck = msg.toLowerCase();
			confirmFormat.forEach((word) => {
				if (msgCheck.indexOf(word) > -1) {
					if (this.datesSelectedFlag === true || this.roomTypeSelectedFlag === true) {
						msgType = 'generalConfirm';
					} else {
						msgType = '';
					}
				}
			});

			// NUMBER CHECK
			const numbers = ['1', '2', '3', '4', '5', '6', '7', '8', '9'];
			let hasNumber = false;
			numbers.forEach((num) => {
				if (msg.indexOf(num) > -1) {
					hasNumber = true;
				}
			});

			// DATE VS ROOM CHECK
			const dateFormat = ['/', '.', '-', 'to'];
			const roomFormat = ['bed', 'beds'];
			let isDate = false;
			let isOneRoom = false;
			let isTwoRoom = false;
			if (hasNumber) {
				//Check if the number is for date confirm or room confirm
				dateFormat.forEach((symbol) => {
					if (msg.indexOf(symbol) > -1) {
						isDate = true;
					}
				});

				roomFormat.forEach((word) => {
					if (msg.indexOf(word) > -1) {
						if (msg.indexOf('1') > -1) {
							isOneRoom = true;
						} else if (msg.indexOf('2') > -1) {
							isTwoRoom = true;
						}
					}
				});
			}

			if (isDate) {
				msgType = 'dateConfirm';
			} else if (isOneRoom) {
				msgType = 'oneConfirm';
			} else if (isTwoRoom) {
				msgType = 'twoConfirm';
			}

			const dateAvailable = Math.floor(Math.random() * 10 + 1);
			const roomAvailable = Math.floor(Math.random() * 10 + 1);

			// Figure out what to do based on the msgType
			switch (msgType) {
				case 'dateConfirm':
					// Important!! Do a check or some kind of random function for if there are rooms available for those dates
					if (dateAvailable <= 3) {
						// If dates not available, prompt again
						this.sendResponseMessage(bot.reply('dateAvail xx01'));
					} else if (this.roomTypeSelectedFlag) {
						this.datesSelectedFlag = true;
						this.sendResponseMessage(bot.reply('dateAvail xx03'));
						this.sendResponseMessage(bot.reply('askForResponse xx01'));
					} else {
						// If dates available, send message and set flag to true
						this.datesSelectedFlag = true;
						this.sendResponseMessage(bot.reply('dateAvail xx02'));
					}
					break;
				case 'suiteConfirm':
					// Important!! Do a check or some kind of random function for which types of rooms available
					// can be all types or nothing, or a combo of which are available and which not
					if (roomAvailable <= 5 && this.datesSelectedFlag === true) {
						this.sendResponseMessage(bot.reply('suiteAvail xx01'));
					} else {
						this.roomTypeSelectedFlag = true;
						this.sendResponseMessage(bot.reply('suiteAvail xx02'));
						this.sendResponseMessage(bot.reply('askForResponse xx01'));
					}
					break;
				case 'twoConfirm':
					// Important!! Do a check or some kind of random function for which types of rooms available
					// can be all types or nothing, or a combo of which are available and which not
					if (roomAvailable <= 5 && this.datesSelectedFlag === true) {
						this.sendResponseMessage(bot.reply('twoAvail xx01'));
					} else {
						this.roomTypeSelectedFlag = true;
						this.sendResponseMessage(bot.reply('twoAvail xx02'));
						this.sendResponseMessage(bot.reply('askForResponse xx01'));
					}
					break;
				case 'oneConfirm':
					// Important!! Do a check or some kind of random function for which types of rooms available
					// can be all types or nothing, or a combo of which are available and which not
					if (roomAvailable <= 5 && this.datesSelectedFlag === true) {
						this.sendResponseMessage(bot.reply('oneAvail xx01'));
					} else {
						this.roomTypeSelectedFlag = true;
						this.sendResponseMessage(bot.reply('oneAvail xx02'));
						this.sendResponseMessage(bot.reply('askForResponse xx01'));
					}
					break;

				case 'generalConfirm':
					// Check the flags for which confirm this applies to and return one of the appropriate next steps

					if (this.roomTypeSelectedFlag && this.datesSelectedFlag) {
						// If date and room type have been confirmed, send a wrap up message and reset everything
						this.sendResponseMessage(bot.reply('generalResponse xx01'));
					} else if (this.datesSelectedFlag === true && this.roomTypeSelectedFlag === false) {
						// If only date, ask for room type
						this.sendResponseMessage(bot.reply('generalResponse xx02'));
					} else {
						// If none or only room type, ask for date
						this.sendResponseMessage(bot.reply('generalResponse xx03'));
					}
					break;
				case 'cancel':
					// Return an appropriate goodbye message and reset (resetEverything function)
					this.resetEverything();
					this.sendResponseMessage(bot.reply('leavingMsg xx01'));
					break;
				default:
					// Return the parsed message the bot gave out
					console.log('default');
					this.sendResponseMessage(botResponse);
					break;
			}
		},
		resetEverything: function() {
			this.datesSelectedFlag = false;
			this.roomTypeSelectedFlag = false;
		},
		initBot: function() {
			// Using a string literal for the script as a last ditch resort
			const scriptString =
				'# Botlang Script' +
				'\n+ "*"' +
				'\n- "I\'m not sure I understand you fully."' +
				'\n- "I\'m sorry, I don\'t understand."' +
				'\n- "Could you rephrase that? I\'m having trouble understanding."' +
				'\n\n+ "(Hello?|Hi|Hey)"' +
				'\n- "Hi! How can I help you?"' +
				'\n- "Hello"' +
				'\n- "Hello, what can I help you with?"' +
				'\n\n+ "How are you?"' +
				'\n- "I\'m doing well, thank you!"' +
				'\n- "I\'m doing well, how about you?"' +
				'\n- "I\'m doing good, how are you?"' +
				'\n- "I\'m fantastic! How are you?"' +
				'\n\n+ "How are you?"' +
				'\n- "I\'m doing well, thank you!"' +
				'\n- "I\'m doing well, how about you?"' +
				'\n- "I\'m doing good, how are you?"' +
				'\n- "I\'m fantastic! How are you?"' +
				'\n\n+ "I\'m doing (well|good|fine|alright|okay|ok) *"' +
				'\n- "Good to hear!"' +
				'\n\n+ "I\'m (well|good|fine|alright|okay|ok) *"' +
				'\n- "Good to hear!"' +
				'\n\n+ "I\'m doing (bad|not good|terrible|) *"' +
				'\n- "Oh no! How can I help you?"' +
				'\n\n+ "My name is $"' +
				'\n- "Nice to meet you $!"' +
				'\n\n+ "Nice to meet you"' +
				'\n- "Nice to meet you as well!"' +
				'\n- "It\'s nice to meet you too!"' +
				'\n\n+ "Where am I?"' +
				'\n- "You\'re on The Bird Bath\'s company website. What can I help you with?"' +
				'\n\n+ "(Thank you|thanks|I appreciate it|thx)"' +
				'\n- "You\'re welcome!"' +
				'\n- "You\'re very welcome."' +
				'\n- "Of course! You\'re welcome!"' +
				'\n\n+ "(your|ur|you\'r) name"' +
				'\n- "My name is Birdie!"' +
				'\n- "My name is Birdie! What\'s yours?"' +
				'\n- "My name is Birdie, what\'s your name?"' +
				'\n\n+ "Help"' +
				'\n- "Alright, tell me how I can help!"' +
				'\n- "Please let me know how I can help!"' +
				'\n- "What can I help you with?"' +
				'\n- "What can I do to help?"' +
				'\n\n+ "(contact|info|human|representative)"' +
				'\n- "Our email is theBirdBath@email.com and phone number is (132) 213 - 1231."' +
				'\n- "Our company email is theBirdBath@email.com and company number is (132) 213 - 1231. How else can I help you?"' +
				'\n\n+ "* (book|reservation) *"' +
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
				'\n\n+ "* (type|kind|style) of (room|unit)"' +
				'\n- "Our room options are a one bed, two bed, and a suite. Availablity varies on the date!"' +
				'\n- "Depending on the time and day, we have a one bed, two bed, and a suite available. All of which are in top quality condiition!"' +
				'\n\n+ "dateAvail xx01"' +
				'\n- "I\'m sorry, there are no available rooms for those dates."' +
				'\n- "I\'m sorry, we\'re all booked those dates."' +
				'\n- "There are no rooms available for those dates, are there any other datet that will work for you?"' +
				'\n- "There are no rooms available for those dates, I\'m sorry!"' +
				'\n\n+ "dateAvail xx02"' +
				'\n- "We have some rooms available, what type were you looking to book?"' +
				'\n- "Alright, we have some rooms available. What kind of room are you looking for?"' +
				'\n\n+ "dateAvail xx03"' +
				'\n- "Its available! I\'ll put you down for that."' +
				'\n- "Sounds good! We\'ll book it down!"' +
				'\n\n+ "* (suite) *"' +
				'\n- "suiteConfirm: SUITE CHECK."' +
				'\n\n+ "suiteAvail xx01"' +
				'\n- "I\'m sorry, our suite is already booked for those dates."' +
				'\n- "Our suite is not available for those dates, is there another date range you\'d be able to book?"' +
				'\n- "Sorry our suite is not available for those dates, is there another room you\'d like to book for those dates?"' +
				'\n- "Sorry, the suite isn\'t available for those dates, are there any other rooms or dates that work for you?"' +
				'\n\n+ "suiteAvail xx02"' +
				'\n- "Good news! Our suite is available! It\'s 100€ a night and has a lovely view."' +
				'\n- "Alright, our suite is available, lets get you booked! It\'s 100€ a night."' +
				'\n- "The suite it is! I just want to confirm with you that It\'s 100€ a night."' +
				'\n- "Can do, you\'ll be booked in the suite, then, at 100€ a night!"' +
				'\n\n+ "* (2|two) bed *"' +
				'\n- "twoConfirm: TWO BED CHECK"' +
				'\n\n+ "twoAvail xx01"' +
				'\n- "Sorry, our two bed rooms are all booked up for those dates. Do you have any other rooms or dates in mind?"' +
				'\n- "Sorry our two bed rooms are not available for those dates, is there another room you\'d like to book for those dates?"' +
				'\n- "I\'m sorry, all of our two bed rooms are already booked for those dates. Are there any other rooms or dates that work for you?"' +
				'\n\n+ "twoAvail xx02"' +
				'\n- "Good news! We have a two bed room available! It\'s 50€ a night and quite comfortable."' +
				'\n- "Alright, there are two bed rooms available, lets get you booked! It\'s 50€ a night."' +
				'\n- "Can do, you\'ll be booked for those dates in a two bed, then, at 50€ a night!"' +
				'\n- "The two bed room is available! I just want to confirm with you that It\'s 50€ a night!. "' +
				'\n\n+ "* (1|one) bed *"' +
				'\n- "oneConfirm: ONE BED CHECK"' +
				'\n\n+ "oneAvail xx01"' +
				'\n- "Sorry, our one bed rooms are all booked up for those dates. Do you have another day or room in mind?"' +
				'\n- "Sorry our one bed rooms are not available for those dates, is there another room you\'d like to book for those dates?"' +
				'\n- "I\'m sorry, all of our one bed rooms are already booked for those dates. Are there any other rooms or dates that work for you?"' +
				'\n\n+ "oneAvail xx02"' +
				'\n- "Good news! We have a one bed room available! It\'s 25€ a night and quite comfortable!"' +
				'\n- "Great news! We have a one bed room available! It\'s 25€ a night. Its very peaceful!"' +
				'\n- "Alright, there are one bed rooms available, lets get you booked! It\'s 25€ a night."' +
				'\n- "Can do, you\'ll be booked in a one bed, then, at 25€ a night!"' +
				'\n\n+ "(goodbye|bye|see ya|cancel|exit)"' +
				'\n- "cancel:  reset flags"' +
				'\n\n+ "leavingMsg xx01"' +
				'\n- "Goodbye. Come back soon!"' +
				'\n- "Bye now. Have a nice day!"' +
				'\n- "Thank you for visiting. Hope we see you soon!"' +
				'\n- "Thanks for visiting The Bird Bath. Have a great day!"' +
				'\n\n+ "* (change|switch|swap) the room"' +
				'\n- "What type of room would you like instead?"' +
				'\n- "Alright, what type of room would you rather have?"' +
				'\n- "Alright, what type of room works better for you?"' +
				'\n\n+ "* (change|switch|swap) room"' +
				'\n- "What type of room would you like instead?"' +
				'\n- "Alright, what type of room would you rather have?"' +
				'\n- "Alright, what type of room works better for you?"' +
				'\n\n+ "* (change|reschedule) the (date|day|dates)"' +
				'\n- "What days work better for you?"' +
				'\n- "When would you like to reschedule your reservation?"' +
				'\n- "Okay, so what days would you like to schedule instead?"' +
				'\n\n+ "* (change|reschedule) (date|day|dates)"' +
				'\n- "What days work better for you?"' +
				'\n- "When would you like to reschedule your reservation?"' +
				'\n- "Okay, so what days would you like to schedule instead?"' +
				'\n\n+ "askForResponse xx01"' +
				'\n- "Okay?"' +
				'\n- "Does this sound like a plan to you?"' +
				'\n- "Is this okay?"' +
				'\n- "Does this work for you?"' +
				'\n- "Does this sound good to you?"' +
				'\n\n+ "generalResponse xx03"' +
				'\n- "Can you tell me the dates you\'re looking to book?"' +
				'\n- "Which days would you like to reserve?"' +
				'\n- "Can I get the dates of the reservation?"' +
				'\n\n+ "generalResponse xx02"' +
				'\n- "What type of room do you prefer?"' +
				'\n- "What kind of room are you looking for?"' +
				'\n- "Can you tell me which room you would like?"' +
				'\n\n+ "generalResponse xx01"' +
				'\n- "Okay, you are all set. Thank you for choosing The Bird Bath! See you soon!"' +
				'\n- "Great! Thank you for choosing The Bird Bath!"' +
				'\n- "Everything sems to be in order. You\'re reservation is complete. Thank you and have a great rest of you\'re day!"' +
				'\n\n+ "(yes|yep|that works|sounds good|confirm)"' +
				'\n- "Alright!"' +
				'\n\n+ "(no|nope|no thank you| no thanks)"' +
				'\n- "We can work this out, how can I help you?"';

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

html {
	::-webkit-scrollbar {
		width: 0px; /* Remove scrollbar space */
		background: transparent; /* Optional: just make scrollbar invisible */
	}
	/* Optional: show position indicator in red */
	::-webkit-scrollbar-thumb {
		background: #ff0000;
	}
}
h1 {
	padding-top: 40px;
	font-size: 84pt;
	color: #0b2027;
	font-family: 'Dancing Script', cursive;
	margin: 0px;
}
h2 {
	font-size: 48pt;
	color: #0b2027;
	font-family: 'Dancing Script', cursive;
	margin: 0 0 0.5em 0;
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
	margin: 0px 40px;
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
	border: 3px solid #40798c;
	border-radius: 15px;
	z-index: 9;
}
/* Add styles to the form container */
.form-container {
	max-width: 450px;
	padding: 15px;
	background-color: white;
	border-radius: 10px;
}
.chats {
	height: 350px;
	border: 2px solid #40798c;
	border-radius: 5px;
	padding: 5px;
	margin-top: 10px;
	background-color: white;
}
.send-container {
	display: flex;
	align-items: center;
}
/* Full-width textarea */
.form-container textarea {
	width: 350px;
	padding: 10px;
	margin-right: 10px;
	margin-top: 10px;
	border: none;
	background: #f1f1f1;
	resize: none;
	display: inline;
	font-size: 12pt;
	border-radius: 5px;

	&:focus {
		background-color: #e6e4e4;
		outline: none;
	}
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
	background-color: #4ebc70;
	color: white;
	padding: 10px;
	border: none;
	text-align: center;
	cursor: pointer;
	width: 60px;
	opacity: 0.8;
	display: inline;
	border-radius: 10px;
	font-weight: bold;
	text-shadow: 1px 1px 2px black;
	letter-spacing: 1px;
}
.chat-title {
	display: inline;
	margin: 20px;
	font-family: 'Open Sans', sans-serif;
	font-size: 16pt;
	color: #40798c;
	text-shadow: 1px 1px 3px #dedede;
}
// MESSAGE CSS - make these able to stretch if longer message?
/* Chat containers */
.message-container {
	border: 2px solid #dedede;
	border-radius: 5px;
	padding: 10px;
	margin: 10px 0;
	min-height: 2.5em;
	box-shadow: 1px 1px 3px #dedede;
}
.darker {
	border-color: #957d959a;
	background-color: #957d9525;
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
	background: white;
}
.covid {
	margin: auto auto;
	padding: 0px 0px 60px 0px;
	font-size: 10pt;
	color: red;
	text-align: center;
	width: 60%;
	font-weight: bold;
}
</style>
