<template>
	<div id="app">
		<div id="nav">
			<router-link to="/">Home |</router-link>
			<router-link to="/"> Reservations </router-link>
			<img src="../public/bird-bath.png" />
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
	padding: 10px;
	padding-bottom: 20px;
	display: flex;
	justify-content: center;
	align-items: center;
	border-bottom: 8px double #957d95;
	width: 101%;
	a {
		font-weight: bold;
		color: #2c3e50;
		text-decoration: none;
		font-size: 18pt;
		word-spacing: 10px;

		&.router-link-exact-active {
			color: #957d95;
		}
	}
	img {
		max-width: 85px;
		margin: 0px 20px;
	}
}
</style>
