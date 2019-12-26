<script>
	import Login from './components/Login.svelte';
	import Interface from './components/Interface.svelte';
	import { onMount } from 'svelte';

	let loggedIN = false;

	let userInfo = {};

	//CHECK IF LOGGED IN
	onMount(async () => {
	const user = JSON.parse(window.localStorage.getItem('devNursery'));
	if(user.username){
		userInfo = user;
		loggedIN = true;
	}
});

const handleLogout = () => {
	window.localStorage.removeItem('devNursery');
	window.localStorage.removeItem('devNtodos');
	loggedIN = false
}

	//FUNCTION FOR LOGIN, WILL PASSED DOWN TO LOGIN COMPONENT
	const loginHandler = async (loginInfo) => {
		console.log(loginInfo);
	    if (loginInfo.username == '' || loginInfo.password == ''){
	        return 0
	    }
	    try {
	        const url = 'https://wrfjnsifxl.execute-api.us-east-1.amazonaws.com/production/api/users/sessions'
	        const response = await fetch(url, {
	            method: 'POST',
	            body: JSON.stringify(loginInfo), // data can be `string` or {object}!
	            headers: {
	                'Content-Type': 'application/json'
	            }
	        });
	        const json = await response.json();
			console.log(json);
			userInfo = json.user;
	        console.log('Success:', JSON.stringify(json));
			loggedIN = true;
			window.localStorage.setItem('devNursery',JSON.stringify(userInfo))

	        } catch (error) {
	            console.error('Error:', error);
	        }
	}
</script>

<main>
	<a href="https://main.devnursery.com"><h1 class="logo"><span>dev</span>Nursery <span>B</span>logging</h1></a>
	{#if !loggedIN}
	<Login loginHandler = {loginHandler}/>
	{/if}
	{#if loggedIN}
	<Interface user={userInfo} logout={handleLogout}/>
	{/if}
	<h3>App Made by Alex Merced of AlexMercedCoder.com using SvelteJS</h3>
</main>

<style>

@import url('https://fonts.googleapis.com/css?family=Major+Mono+Display&display=swap');

main {padding: 20px; text-align: center}

main a {text-decoration: none; color: black;}

.logo {text-align: center; font-size: 300%; width: 100%; margin: 10px auto; font-family: 'Major Mono Display', monospace;}

.logo span {color: #A5243D;}

@media only screen and (max-width: 500px) {
  .logo {font-size: 100%}
}
</style>
