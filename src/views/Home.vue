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
						<p class="reply">Hi, I'm ChatBot! How can I help you today?</p>
					</div>

					<!-- <div class="message-container darker">
						<img src="../../public/bird.png" alt="Avatar" class="user avatar" />
						<p class="message">Hey! I'm fine. Thanks for asking!</p>
					</div> -->
				</div>
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
//import * as fs from "fs";
import Botlang from 'botlang';

export default {
	data(){
		return{
			msg: '',
			lastMsgType: '',
			datesSelectedFlag: false,
			roomTypeSelectedFlag: false
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
		sendUserMessage: function(msg: string){
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
		sendResponseMessage: function(msg: string){
			// Send the message bubble into the chat
			const messageBubble = document.querySelector('.chats') as HTMLElement;
			if(messageBubble){
				messageBubble.innerHTML += "<div class='message-container'>"
						+ "<img src='/bird.png' alt='Avatar' class='ai avatar' />"
						+ "<p class='reply'>" + msg + "</p></div>"
			}
		},
		parseMessage:function (msg: string){
			// Initialize the language bot
			const bot = this.initBot();

			// Get the response and parse it out
			const botResponse = bot.reply(msg);
			console.log(botResponse)
			const msgType = botResponse.split(":")[0];
			const parsedMessage = botResponse.split(":")[1]; // Uncomment me when new format has been applied to script
			const roomType = botResponse.split(":")[2];
			console.log(botResponse)

			const avail = this.availRand();

			// this.lastMsgType = msgType;

			// Figure out what to do based on the msgType
			switch(msgType){
				case "greeting":
					// Not sure what we can do with this one - maybe it doesn't need its own category
					this.sendResponseMessage(parsedMessage);
					break;
				case "dateConfirm":
					// Important!! Do a check or some kind of random function for if there are rooms available for those dates
					// If dates not available, prompt again
					// If dates available, send already parsed message and set flag to true

					// if(this.datesSelectedFlag){
						//select confirmation repeat response
					// }else{
						if(avail){
							
							// this.datesSelectedFlag = true;
							this.sendResponseMessage(parsedMessage);
						}else{
							const newBotResponse = bot.reply("dateNotAvail xxx2341");
							this.sendResponseMessage(newBotResponse);
						}
					// }
					break;
				case "roomTypeConfirm":
					// Important!! Do a check or some kind of random function for which types of rooms available -
					// can be all types or nothing, or a combo of which are available and which not
									
					// if(this.roomTypeSelectedFlag){
						//send room type repeat response
					// }else{
						switch(roomType){
						case "suite":
							if(avail){
								// this.roomTypeSelectedFlag = true;
								this.sendResponseMessage(parsedMessage);
							}else{
								const newBotResponse = bot.reply("suiteNotAvail xxx2341");
								this.sendResponseMessage(newBotResponse);
							}
							break;
						case "two":
							if(avail){
								// this.roomTypeSelectedFlag = true;
								this.sendResponseMessage(parsedMessage);
							}else{
								const newBotResponse = bot.reply("twoBedNotAvail xxx2341");
								this.sendResponseMessage(newBotResponse);
							}
							break;
						case "one":
							if(avail){
								// this.roomTypeSelectedFlag = true;
								this.sendResponseMessage(parsedMessage);
							}else{
								const newBotResponse = bot.reply("oneBedNotAvail xxx2341");
								this.sendResponseMessage(newBotResponse);
							}
							break;
						default:
							console.log("ERR: Missing room type tag at end of bot response");
							
							break;
						}
					// }							
				
					break;
				case "generalConfirm":	

					
				
					// Check the flags for which confirm this applies to and return one of the appropriate next steps
					// If date and room type have been confirmed, send a wrap up message and reset everything
					// If only date, ask for room type
					// If none or only room type, ask for date

					// if(this.roomTypeSelectedFlag && this.datesSelectedFlag){
					// 	this.sendResponseMessage("finalResponse xxx2341");
					// 	this.resetEverything();

					// }else if(this.roomTypeSelectedFlag && !this.datesSelectedFlag){
					// 	this.sendResponseMessage("confirmDateAgain xxx2341");

					// }else if(!this.roomTypeSelectedFlag && this.datesSelectedFlag){
					// 	this.sendResponseMessage("confirmRoomAgain xxx2341");
					// }
				
					break;
				case "changeRoom":
					// this.roomTypeSelectedFlag = false;
					break;
				case "changeDate":
					// this.datesSelectedFlag = false;

					break;	

				case "cancel":
					// Return an appropriate goodbye message and reset (resetEverything function)
					this.sendResponseMessage(parsedMessage);
					this.resetEverything();
					break;
				default: // aka case other
					// Return the parsed message the bot gave out
					this.sendResponseMessage(parsedMessage);

					break;
			}

			// Send response
			// this.sendResponseMessage(parsedMessage);
		},
		resetEverything:function(){
			// this.datesSelectedFlag = false;
			// this.roomTypeSelectedFlag = false;
			console.log("reset")
		},

		availRand:function(){
			const rand = Math.floor(Math.random() * Math.floor(3));
			if(rand != 2){
				return true;
			}
			return false;
		},

		initBot:function(){
			// Using a string literal for the script as a last ditch resort
			const scriptString = '# Botlang Script' + '\n+ "*"' +
			'\n- "other: I\'m not sure I understand you fully."' +
			'\n- "other: I\'m sorry, I don\'t understand."' +
			'\n- "other: Could you rephrase that? I\'m having trouble understanding."' +

			'\n\n+ "Hello?"' +
			'\n- "greeting: Hi! How can I help you?"' +
			'\n- "greeting: Hello!"' +

			'\n\n+ "How are you?"' +
			'\n- "other: I\'m doing well, thank you!"' +
			'\n- "other: I\'m doing well, how about you?"' +
			'\n- "other: I\'m doing good, how are you?"' +
			'\n- "other: I\'m fantastic! How are you?"' +

			'\n\n+ "Thank you!"' +
			'\n- "other: You\'re welcome!"' +
			'\n- "other: You\'re very welcome."' +
			'\n- "other: Of course! You\'re welcome!"' +
			'\n- "other: Anytime, You\'re welcome!"' +

			'\n\n+ "Help"' +
			'\n- "other: Alright, tell me how I can help!"' +
			'\n- "other: Please let me know how I can help!"' +
			'\n- "other: What can I help you with?"' +

			'\n\n+ "* (book|reservation|room) *"' +
			'\n- "other: Do you have a date range?"' +
			'\n- "other: And what dates are you looking to book?"' +
			'\n- "other: Okay! When would your stay be?"' +
			'\n- "other: Can I get the dates You\'re looking to book?"' +
			'\n- "other: What are the beginning and end dates of your stay?"' +
			'\n- "other: Can do! What dates are you looking to book?"' +

			'\n\n+ "* (book|make) reservation"' +
			'\n- "other: Can I get the dates of the reservation?"' +
			'\n- "other: And what are the dates you are looking to reserve?"' +
			'\n- "other: Okay! Can I get the dates of the reservation you\'d like to make?"' +
			'\n- "other: Awesome! When are you thinking about booking?"' +
			'\n- "other: Ok what dates are you looking to book?"' +

			'\n\n# if no rooms available' +
			'\n+ "dateNotAvail xxx2341"' +
			'\n- "I\'m sorry, there are no available rooms for those dates."' +
			'\n- "I\'m sorry, we\'re all booked those dates. Would you like to try another date?"' +
			'\n- "There are no rooms available for those dates, are there any other dates that will work for you?"' +
			'\n- "There are no rooms available for those dates, I\'m sorry!"' +

			'\n\n# if all rooms available' + 
			'\n+ "* to *"' +
			'\n- "dateConfirm: We have a one bed, two bed, or the suite available for those days. Which room would you prefer?"' +

			'\n\n# if suite is not available' +
			'\n+ "suiteNotAvail xxx2341"' +
			'\n- "I\'m sorry, our suite is already booked for those dates. Let\'s try another date."' +
			'\n- "Our suite is not available for those dates, is there another date range you\'d be able to book?"' +
			'\n- "Sorry, the suite isn\'t available for those dates, are there any other rooms or dates that work for you?"' +

			'\n\n# if suite is available' +
			'\n+ "* (suite) *"' +
			'\n- "roomTypeConfirm: Good news! Our suite is available for those dates! It\'s 150€ a night and has a lovely view. Does that work for you?:suite"' +
			'\n- "roomTypeConfirm: Alright, our suite is available for those dates, lets get you booked! It\'s 150€ a night, is that alright?:suite"' +
			'\n- "roomTypeConfirm: The suite it is! I just want to confirm with you that It\'s 150€ a night.:suite"' +
			'\n- "roomTypeConfirm: Can do, you\'ll be booked for those dates in the suite, then, at 150€ a night! Okay?:suite"' +

			'\n\n+ "* (suite)"' +
			'\n- "roomTypeConfirm: Good news! Our suite is available for those dates! It\'s 150€ a night and has a lovely view. Does that work for you?:suite"' +
			'\n- "roomTypeConfirm: Alright, our suite is available for those dates, lets get you booked! It\'s 150€ a night, is that alright?:suite"' +
			'\n- "roomTypeConfirm: The suite it is! I just want to confirm with you that It\'s 150€ a night.:suite"' +
			'\n- "roomTypeConfirm: Can do, you\'ll be booked for those dates in the suite, then, at 150€ a night!:suite"' +

			'\n\n# if two bed is available' +
			'\n+ "* (2|two) bed *"' +
			'\n- "roomTypeConfirm: Good news! We have a two bed room available for those dates! It\'s 100€ a night and quite comfortable. Sound good?:two"' +
			'\n- "roomTypeConfirm: Alright, there are two bed rooms available for those dates, lets get you booked! It\'s 100€ a night, is that alright?:two"' +
			'\n- "roomTypeConfirm: Can do, you\'ll be booked for those dates in a two bed, then, at 100€ a night! Okay?:two"' +

			'\n\n+ "*(2|two) bed"' +
			'\n- "roomTypeConfirm: Good news! We have a two bed room available for those dates! It\'s 100€ a night and quite comfortable. Sound good?:two"' +
			'\n- "roomTypeConfirm: Alright, our suite is available for those dates, lets get you booked! It\'s 100€ a night, is that alright?:two"' +
			'\n- "roomTypeConfirm: The suite it is! I just want to confirm with you that It\'s 100€ a night.:two"' +
			'\n- "roomTypeConfirm: Can do, you\'ll be booked for those dates in the suite then at 100€ a night! Okay?:two"' +

			'\n\n# if two bed is not available' +
			'\n+ "twoBedNotAvail xxx2341"' +
			'\n- "Sorry, our two bed rooms are all booked up for those dates. Do you have any other rooms or dates in mind?"' +
			'\n- "I\'m sorry, all of our two bed rooms are already booked for those dates. Are there any other rooms or dates that work for you?"' +
			
			'\n\n# if one bed is available' +
			'\n+ "* (1|one) bed *"' +
			'\n- "roomTypeConfirm: Good news! We have a one bed room available for those dates! It\'s 50€ a night and very peaceful.:one"' +
			'\n- "roomTypeConfirm: Alright, there is a one bed room available for those dates, lets get you booked! It\'s $50€ a night, is that alright?:one"' +
			'\n- "roomTypeConfirm: Can do, you\'ll be booked for those dates in a one bed, then, at $50€ a night!:one"' +

			'\n+ "* (1|one) bed" ' +
			'\n- "roomTypeConfirm: Good news! We have a one bed room available for those dates! It\'s $50€ a night and very peaceful.:one"' +
			'\n- "roomTypeConfirm: Alright, there is a one bed room available for those dates, lets get you booked! It\'s $50€ a night, is that alright?:one"' +
			'\n- "roomTypeConfirm: Can do, you\'ll be booked for those dates in a one bed, then, at $50€ a night!:one"' +
			
			'\n\n# if one bed is not available' +
			'\n+ "oneBedNotAvail xxx2341"' +
			'\n- "I\'m sorry, all of our one bed rooms are already booked for those dates. Are there any other rooms or dates that work for you?"' +
			'\n- "Sorry, our one bed rooms are all booked up for those dates. Do you have any other rooms or dates in mind?"' +

			'\n\n+ "(goodbye|bye|see ya|nevermind)"' +
			'\n- "cancel: Goodbye. Come back soon!"' +
			'\n- "cancel: Bye now. Have a nice day!"' +
			'\n- "cancel: Thank you for visiting. Hope we see you soon!"' +
			
			'\n\n+ "* (change|switch|swap) room"' +
			'\n- "changeRoom: What type of room would you like instead?"' +
			'\n- "changeRoom: Alright, what type of room works better fore you?"' +

			'\n\n+ "* (change|reschedule) the (date|day)"' +
			'\n- "changeDate: What days work better for you?"' +
			'\n- "changeDate: Okay, so what days would you like to schedule instead?"' +
			
			'\n\n+ "(yes|yep|that works|sounds good)"' +
			'\n- "generalConfirm: Okay"' +
			'\n- "generalConfirm: Great!"' +
			'\n- "generalConfirm: You got it!"' +
			
			'\n\n "confirmRoomAgain xxx2341"' +
			'"What type of room do you prefer?"' +
			'"What kind of room are you looking for?"' + 
			'"Can you tell me which room you would like?"' +
			
			'\n\n "confirmDateAgain xxx2341"' +
			'"Can you tell me the dates you\'re looking to book?"' +
			'"Which days would you like to reserve?"' +
			'"Can I get the dates of the reservation?"' + 
			
			'\n\n "finalResponse xxx2341"' +
			'"Okay, you are all set. Thank you for choosing the The Bird Bath! See you soon!"' +
			'"Everything sems to be in order. You\'re reservation is complete. Thank you and have a great rest of you\'re day!"';



	
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
