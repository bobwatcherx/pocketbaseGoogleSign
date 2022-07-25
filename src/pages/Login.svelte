<script >
	import PocketBase from 'pocketbase';
const client = new PocketBase('http://localhost:8090');

let liststate = []
let redirecturl = "http://localhost:5173/redirect" 
// redirecturl GET FROM CONSOLE.google.com 
async function listalllogin(){
	const result = await client.Users.listAuthMethods();
	console.log(result)
	liststate = result.authProviders
}
listalllogin()
async function googlesign(){
	const local = localStorage.setItem("provider",JSON.stringify(liststate))
	window.location.href = liststate[0].authUrl + redirecturl
}
</script>

<div>
	<h1>Login page</h1>
	<br/>
{#each liststate as l}
	<button
	on:click={googlesign}
	>{l.name}</button>
{/each}
</div>