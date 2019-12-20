<script>
	import Login from './components/Login.svelte'
	let loggedIN = false;
	let userInfo = [];
</script>

<main>
	<h1 class="logo"><span>dev</span>Nursery <span>B</span>logging</h1>
	<Login/>
</main>

<style>

@import url('https://fonts.googleapis.com/css?family=Major+Mono+Display&display=swap');

main {padding: 100px;}

.logo {text-align: center; font-size: 300%; width: 100%; margin: 10px auto; font-family: 'Major Mono Display', monospace;}

.logo span {color: #A5243D;}
</style>
