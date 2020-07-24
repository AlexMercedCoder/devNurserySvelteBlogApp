<script>
import { onMount } from 'svelte';

export let user;

export let logout;

let switches = {};

const apiURL = true ? "https://devnurseryapi.herokuapp.com" : "http://localhost:3000"

let newNote = {
    title: '',
    body: '',
    tech: '',
    user: user.username
};

let editProps = {};

let createPost = false;

let noteArray = [];

//Toggles Create Post Form
const showCreate = () => createPost = !createPost;

//Gets Posts from LocalStorage, if none, checks the api
onMount(async () => {
        if (window.localStorage.getItem('devNurseryPosts')){
            noteArray = JSON.parse(window.localStorage.getItem('devNurseryPosts'));
            console.log('from localstorage')
        } else {
            console.log(user);
            console.log(newNote)
    		const res = await fetch(`${apiURL}/note`, {
                method: 'get',
                headers: {
                    Authorization: `bearer ${user.token}`
                }
            });
    		noteArray = await res.json();
            console.log(noteArray)
        }

	});

//Submits post to API, adds to noteArray and stores it in localstorage
const submitPost = async () => {
    try {
        const url = `${apiURL}/note`
        const response = await fetch(url, {
            method: 'POST',
            body: JSON.stringify(newNote), // data can be `string` or {object}!
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `bearer ${user.token}`
            }
        });
        const json = await response.json();
        console.log(json);
        noteArray = [...noteArray,json];
        console.log('Success:', JSON.stringify(json));
        window.localStorage.setItem('devNurseryPosts',JSON.stringify(noteArray))
        newNote = {
            title: '',
            body: '',
            user: user.username
        };

        } catch (error) {
            console.error('Error:', error);
        }

}

//Edits post, edits array, stores in local storage
const editPost = async (event, id, index) => {
    try {
        console.log(editProps)
        const bodyObj = {username: user.username};
        if (editProps[id+'title']){
            bodyObj.title = editProps[id+'title'];
        }
        if (editProps[id+'entry']){
            bodyObj.body = editProps[id+'body'];
        }
        console.log(bodyObj);
        console.log(id);
        console.log(index);
        const url = `${apiURL}/note/${id}`;
        const response = await fetch(url, {
            method: 'PUT',
            body: JSON.stringify(bodyObj), // data can be `string` or {object}!
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `bearer ${user.token}`
            }
        });
        const json = await response.json();
        console.log(json);
        noteArray[index] = json;
        console.log('Success:', JSON.stringify(json));
        window.localStorage.setItem('devNurseryPosts',JSON.stringify(noteArray));
        } catch (error) {
            console.error('Error:', error);
        }

}

//Deletes Post, Splices item from array, stores in localstorage
const deletePost = async (id,index) => {
    console.log(id + " " + index)
    const url = `${apiURL}/note/${id}`
    const response = await fetch(url, {
        method: 'DELETE',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': `bearer ${user.token}`
        }
    });
    const json = await response.json();
    noteArray.splice(index,1);
    window.localStorage.setItem('devNurseryPosts',JSON.stringify(noteArray));
    noteArray = [...noteArray] //assignment to trigger DOM Changes
    console.log(noteArray)
}

</script>

<div class="interface">
    <h1> Welcome {user.username} </h1>
    <p>WARNING: This site is currently in early testing, so the database may be cleared to install additional features. Make sure to backup your notes on websites like notion.so or trello. Please do not use this feature to store anything sensitive.</p>
    <nav>
        <button on:click={showCreate}>Create Post</button>
        <button on:click={logout}>Logout</button>
        </nav>
    <div>


    {#if createPost}
    <form on:submit|preventDefault={submitPost}>
    <input type='input' placeholder='title' bind:value={newNote.title}/>
    <textarea type='input' placeholder='body' bind:value={newNote.body}/>
    <input type='submit' value='submit post'/>
    </form>
    {/if}

    <div>
    {#each noteArray as post, index}
    <div>
    <h1>{post.title}</h1>
    <p>{post.body}</p>
    <button on:click={e => deletePost(post._id,index)}>Delete Post</button>
    <button on:click={e => switches[post._id] = !switches[post._id]}>Edit Post</button>
    </div>
    {#if switches[post._id]}
    <div>
    <form on:submit|preventDefault={e => editPost(e,post._id,index)}>
    <input type='input' placeholder='title' bind:value={editProps[post._id +'title']} name="title"/>
    <textarea type='input' placeholder='body' bind:value={editProps[post._id +'body']} name="body"/>
    <input type='submit' value='submit post'/>
    </form>
    </div>
    {/if}
    {/each}

    </div>


    </div>
</div>

<style>
.interface {width: 80%; margin: 10px auto; padding: 10px; text-align: center; border: 3px solid #A5243D}

nav {display: flex; justify-content: space-around;}

button, input, textarea { width: 300px; border: none; background-color: #A5243D; color: white; padding: 10px; margin: 5px;
        transition: background-color .4s, color .4s,border .4s;}

button:hover, input:hover, textarea:hover {background-color: #B9CFDF; color: black; border: 3px solid #A5243D}

form {display: flex; flex-direction: column; margin: 20px auto; justify-content: center; align-items: center;}

@media only screen and (max-width: 500px) {
  button, input, textarea {width: 70%;}
}

</style>
