<script>
import { onMount } from 'svelte';

export let user;

export let logout;

let switches = {};

let newPost = {
    title: '',
    entry: '',
    tech: '',
    user: user.username
};

let editProps = {};

let createPost = false;

let postArray = [];

//Toggles Create Post Form
const showCreate = () => createPost = !createPost;

//Gets Posts from LocalStorage, if none, checks the api
onMount(async () => {
        if (window.localStorage.getItem('devNurseryPosts')){
            postArray = JSON.parse(window.localStorage.getItem('devNurseryPosts'));
            console.log('from localstorage')
        } else {
            console.log(user);
            console.log(newPost)
    		const res = await fetch(`https://wrfjnsifxl.execute-api.us-east-1.amazonaws.com/production/api/blogs/${user.username}`);
    		postArray = await res.json();
            console.log(postArray)
        }

	});

//Submits post to API, adds to postArray and stores it in localstorage
const submitPost = async () => {
    try {
        const url = 'https://wrfjnsifxl.execute-api.us-east-1.amazonaws.com/production/api/blogs/'
        const response = await fetch(url, {
            method: 'POST',
            body: JSON.stringify(newPost), // data can be `string` or {object}!
            headers: {
                'Content-Type': 'application/json'
            }
        });
        const json = await response.json();
        console.log(json);
        postArray = [...postArray,json];
        console.log('Success:', JSON.stringify(json));
        window.localStorage.setItem('devNurseryPosts',JSON.stringify(postArray))
        newPost = {
            title: '',
            entry: '',
            tech: '',
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
        const bodyObj = {user: user.username};
        if (editProps[id+'title']){
            bodyObj.title = editProps[id+'title'];
        }
        if (editProps[id+'entry']){
            bodyObj.entry = editProps[id+'entry'];
        }
        if (editProps[id+'tech']){
            bodyObj.tech = editProps[id+'tech'];
        }
        console.log(bodyObj);
        console.log(id);
        console.log(index);
        const url = `https://wrfjnsifxl.execute-api.us-east-1.amazonaws.com/production/api/blogs/${id}`;
        const response = await fetch(url, {
            method: 'PUT',
            body: JSON.stringify(bodyObj), // data can be `string` or {object}!
            headers: {
                'Content-Type': 'application/json'
            }
        });
        const json = await response.json();
        console.log(json);
        postArray[index] = json;
        console.log('Success:', JSON.stringify(json));
        window.localStorage.setItem('devNurseryPosts',JSON.stringify(postArray));
        } catch (error) {
            console.error('Error:', error);
        }

}

//Deletes Post, Splices item from array, stores in localstorage
const deletePost = async (id,index) => {
    console.log(id + " " + index)
    const url = `https://wrfjnsifxl.execute-api.us-east-1.amazonaws.com/production/api/blogs/${id}`
    const response = await fetch(url, {
        method: 'DELETE',
        body: JSON.stringify(newPost), // data can be `string` or {object}!
        headers: {
            'Content-Type': 'application/json'
        }
    });
    const json = await response.json();
    postArray.splice(index,1);
    window.localStorage.setItem('devNurseryPosts',JSON.stringify(postArray));
    postArray = [...postArray] //assignment to trigger DOM Changes
    console.log(postArray)
}

</script>

<div class="interface">
    <h1> Welcome {user.username} </h1>
    <p>WARNING: This site is currently in early testing, so the database may be cleared to install additional features. Make sure to backup your posts on websites like Dev.to or Medium.</p>
    <p>Each post has a title, body and section to list the tech you are focusing on. You can find your public blog page at http://blogs.devnursery.com/[username]</p>
    <nav>
        <button on:click={showCreate}>Create Post</button>
        <button on:click={logout}>Logout</button>
        </nav>
    <div>


    {#if createPost}
    <form on:submit|preventDefault={submitPost}>
    <input type='input' placeholder='title' bind:value={newPost.title}/>
    <textarea type='input' placeholder='entry' bind:value={newPost.entry}/>
    <input type='input' placeholder='tech highlighted' bind:value={newPost.tech}/>
    <input type='submit' value='submit post'/>
    </form>
    {/if}

    <div>
    {#each postArray as post, index}
    <div>
    <h1>{post.title}</h1>
    <p>{post.entry}</p>
    <h3>{post.tech}</h3>
    <h3>{post.Date}</h3>
    <button on:click={e => deletePost(post._id,index)}>Delete Post</button>
    <button on:click={e => switches[post._id] = !switches[post._id]}>Edit Post</button>
    </div>
    {#if switches[post._id]}
    <div>
    <form on:submit|preventDefault={e => editPost(e,post._id,index)}>
    <input type='input' placeholder='title' bind:value={editProps[post._id +'title']} name="title"/>
    <textarea type='input' placeholder='entry' bind:value={editProps[post._id +'entry']} name="entry"/>
    <input type='input' placeholder='tech highlighted' bind:value={editProps[post._id +'tech']} name="tech"/>
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
