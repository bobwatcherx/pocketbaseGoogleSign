<script >
	import PocketBase from 'pocketbase';
const client = new PocketBase('http://localhost:8090');
 const params = (new URL(window.location)).searchParams;
// params for get url code from url bar
let redirecturl = "http://localhost:5173/redirect"
const getoken = localStorage.getItem('provider')
console.log("getoken" + getoken)
 let cekprovider = ""
 if(getoken){
 	cekprovider = JSON.parse(getoken)
 }
let pleasewait = false
let content = false
console.log(cekprovider)
async function usertogoogle(){
	const authData = await client.Users.authViaOAuth2(
		cekprovider[0].name,//THIS VALUE IS google get from localstorage
		  params.get("code"),
		 cekprovider[0].codeVerifier,//CEK YOU LOCALSTORAGE
		  redirecturl
		 ).then((authdata)=>{
		 	console.log(authdata)
		 	content = true 
		 	pleasewait = false
		 }).catch(err=>console.log(err))
}
// CONDITION IF YOUR localStorage NOT FOUND PROVIDER VALUE
// YOU AUTOMATICALLY REDIRECT TO /LOGIN
  if (getoken == "" && cekprovider == "" ) {
        window.location.href = "/"
        pleasewait = true
      }
// IF cekprovider is FOUND IN YOUR localStorage
// run function usertogoogle
if(cekprovider !== null){
	console.log(cekprovider)
	// IF NOT PARAMS YOU WILL REDIRECT TO LOGIN PAGE
	if(cekprovider[0].state !== params.get("state")){
		pleasewait = true
		window.location.href = "/"
	}
	else{
		usertogoogle()
	}

}
</script>

<div>
	{#if pleasewait == true}
	<h1>SORRY YOU MUST LOGIN , BACK TO LOGIN</h1>
	{/if}
	<!-- IF SUCCESS YOU WELCOME PAGE -->
	{#if content == true}
	<h1>WELCOME TO YOU DASHBOARD</h1>
	<button
	on:click={()=>{
		// REMOVE LOCALSTORAGE CLEAN PROVIDER
		localStorage.removeItem('provider')
		// AND REDIRECT TO BACK LOGIN AGAIN
		window.location.href = "/"
	}}
	>Logout</button>

	{/if}
	
	</div>	
