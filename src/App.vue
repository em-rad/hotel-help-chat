<template>
	<div id="app">
		<div id="nav">
			<img src="../public/bird-bath.png" />
			<router-link to="/">Home |</router-link>
			<router-link to="/"> Reservations |</router-link>
			<router-link to="/"> Deals |</router-link>
			<router-link to="/"> Contact</router-link>
		</div>
		<router-view />
	</div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');
#app {
	font-family: 'Open Sans', sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
}

#nav {
	padding: 30px;
	border-bottom: 8px double #b8a57f;
	width: 101%;
	a {
		font-weight: bold;
		color: #2c3e50;
		text-decoration: none;
		font-size: 18pt;
		word-spacing: 10px;

		&.router-link-exact-active {
			color: #b8a57f;
		}
	}
	img {
		max-width: 85px;
		position: absolute;
		left: 30px;
		top: 12px;
	}
}
</style>
