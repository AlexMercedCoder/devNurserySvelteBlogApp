<script>
export let loginStatus
</script>

<div class="login">

<h2>Login</h2>

<form>
    <h3>username</h3>
    <input type="text" name="username"/>
    <h3>password</h3>
    <input type="password" name="password"/>
    <input type="submit" value="Login"/>
</form>

{#if userInfo}

<button>Create Account</button>



</div>


<style>

@import url('https://fonts.googleapis.com/css?family=Manjari|Staatliches&display=swap');

.login {width: 300px; height: 300px; margin: 30px auto; background-color: #B9CFDF; text-align: center; padding: 10px; font-family: 'Manjari', sans-serif;}

input { width: 60%; border: none; background-color: #A5243D; color: white; padding: 10px; margin: 5px;}

h1,h2,h3,h4,h5,h6 {font-family: 'Staatliches', cursive;}

</style>
