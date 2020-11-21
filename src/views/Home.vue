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
			<img class="examples" src="../..//public/room1.jpg" />
			<img class="examples" src="../..//public/room2.jpg" />
			<img class="examples" src="../..//public/bath.jpg" />
		</div>
		<button class="book-now">BOOK NOW</button>

		<button class="open-button" @click="openForm()">Chat</button>

		<div class="chat-popup" id="myForm">
			<form class="form-container">
				<h3>Customer Service Chat</h3>

				<label for="msg"><b>Message</b></label>
				<!-- TODO when message is sent, create new element and add child into message div -->
				<textarea placeholder="Type message..." name="msg" required></textarea>
				<!-- TODO change message button to be handled by 'Enter' -->
				<button type="submit" class="btn">Send</button>
				<!-- TODO change cancel button to X in top right -->
				<button type="button" class="btn cancel" @click="closeForm()">Close</button>
			</form>
		</div>
		<footer></footer>
	</div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

export default {
	methods: {
		openForm() {
			const form = document.querySelector('#myForm') as HTMLElement;
			form.style.display = 'block';
		},
		closeForm() {
			const form = document.querySelector('#myForm') as HTMLElement;
			form.style.display = 'none';
		},
	},
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
.image-container {
	display: flex;
	justify-content: space-between;
	margin: 0 40px 0 40px;
}
.examples {
	width: 400px;
	height: 400px;
	border: 10px solid #957d95;
}

/* Button used to open the chat form - fixed at the bottom of the page */
.open-button {
	background-color: #555;
	color: white;
	padding: 16px 20px;
	border: none;
	cursor: pointer;
	opacity: 0.8;
	position: fixed;
	bottom: 23px;
	right: 28px;
	width: 280px;
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
	max-width: 300px;
	padding: 10px;
	background-color: white;
}

/* Full-width textarea */
.form-container textarea {
	width: 100%;
	padding: 15px;
	margin: 5px 0 22px 0;
	border: none;
	background: #f1f1f1;
	resize: none;
	min-height: 200px;
}

/* When the textarea gets focus, do something */
.form-container textarea:focus {
	background-color: #ddd;
	outline: none;
}

/* Set a style for the submit/send button */
.form-container .btn {
	background-color: #4caf50;
	color: white;
	padding: 16px 20px;
	border: none;
	cursor: pointer;
	width: 100%;
	margin-bottom: 10px;
	opacity: 0.8;
}

/* Add a red background color to the cancel button */
.form-container .cancel {
	background-color: red;
}

/* Add some hover effects to buttons */
.form-container .btn:hover,
.open-button:hover {
	opacity: 1;
}
</style>
