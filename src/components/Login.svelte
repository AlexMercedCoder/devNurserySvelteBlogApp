<script>
export let loginHandler;

let makeAccount = false;

const apiURL = true ? "https://devnurseryapi.herokuapp.com" : "http://localhost:3000"

const login = (e) => {
    console.log(loginInfo);
    loginHandler(loginInfo);}

const showCreate = () => {makeAccount = !makeAccount};

let newAccount = {
    username: '',
    password: '',
};

let loginInfo = {
    username: '',
    password: ''
}

let createMessage = 'Fill Form Below';

//FUNCTION TO CREATE A NEW ACCOUNT
const handleCreate = async () => {
    console.log(newAccount);
    if (newAccount.username == '' || newAccount.password == ''){
        createMessage = 'Failed: Need Username and Password'
        return 0
    }
    try {
        const url = `${apiURL}/user/create`
        const response = await fetch(url, {
            method: 'POST',
            body: JSON.stringify(newAccount), // data can be `string` or {object}!
            headers: {
                'Content-Type': 'application/json'
            }
        });
        const json = await response.json();
        createMessage = 'Success, now login above!'
        console.log('Success:', JSON.stringify(json));
        } catch (error) {
            createMessage = error;
            console.error('Error:', error);
        }
}


</script>

<div>
    <div class="login">
    <h1>Login</h1>

    <form on:submit|preventDefault = {login}>
        <h2>username</h2>
        <input type="text" name="username" bind:value={loginInfo.username}/>
        <h2>password</h2>
        <input type="password" name="password" bind:value={loginInfo.password}/>
        <input type="submit" value="Login"/>
    </form>
    </div>

    <div class="login">
        <button on:click={showCreate}>Create Account</button>

        {#if makeAccount}
        <h2>{createMessage}</h2>
        <h2> New Account </h2>
        <form on:submit|preventDefault ={handleCreate}>
            <input type="text" placeholder="username" name="username" bind:value={newAccount.username}/>
            <input type="password" placeholder="password" name="password" bind:value={newAccount.password}/>
            <input type="submit" value="Create User"/>
        </form>
        {/if}
    </div>

</div>


<style>

@import url('https://fonts.googleapis.com/css?family=Manjari|Staatliches&display=swap');

.login {width: 320px; margin: 30px auto; background-color: #B9CFDF; text-align: center; padding: 10px; font-family: 'Manjari', sans-serif;}

input, button { width: 60%; border: none; background-color: #A5243D; color: white; padding: 10px; margin: 5px;
    transition: background-color .4s, color .4s, border .4s;}

input:hover, button:hover {background-color: #B9CFDF; color: black; border: 3px solid #A5243D}
h1,h2,h3,h4,h5,h6 {font-family: 'Staatliches', cursive;}

</style>
