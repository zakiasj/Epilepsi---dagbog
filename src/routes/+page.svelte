<script>
  import { goto } from '$app/navigation';
  import Title from '$lib/components/Title.svelte';
  import Link from '$lib/components/Link.svelte';
  import H1 from '$lib/components/H1.svelte';

  let username = '';
  let password = '';

  const login = async () => {
    const response = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username, password })
    });
    const data = await response.json();
    if (response.ok) {
      goto('/diary');
    } else {
      alert('Login mislykkedes. Prøv igen.');
    }
  };
</script>

<style>
  body {
    height: 100vh;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #e0e0e0; /* Grå baggrund */
    font-family: Arial, sans-serif;
  }

  .full-page-container {
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    padding: 20px;
  }

  .login-container {
    padding: 20px;
    background-color: white;
    border-radius: 8px;
    width: 100%;
    max-width: 400px;
    font-family: Arial, sans-serif;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
  }

  .input {
    width: 100%;
    padding: 12px;
    margin: 12px 0;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
  }

  .btn {
    width: 100%;
    padding: 14px;
    background-color: red;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .btn:hover {
    background-color: darkred;
  }

  .link {
    margin-top: 10px;
    color: red;
    text-decoration: none;
    font-size: 14px;
  }

  .link:hover {
    text-decoration: underline;
  }

  .title {
    font-size: 2rem;
    font-weight: bold;
    color: red; /* Velkomsttekst i rød */
  }

  .subtitle {
    font-size: 1rem;
    color: #000; /* Sort farve til undertekst */
    margin-bottom: 20px;
  }
</style>

<div class="full-page-container">
  <div class="login-container">
    <Title class="title">Velkommen til epilepsi dagbogen</Title>
    <H1 class="subtitle">Log ind for at registrere dine anfald og se din historik</H1>

    <input class="input" bind:value={username} type="text" placeholder="Indtast Email" />
    <input class="input" bind:value={password} type="password" placeholder="Indtast Password" />
    <button class="btn" on:click={login}>Login</button>

    <p>Har du ikke en konto endnu?</p>
    <Link class="link" href="/signup">Opret ny konto</Link>
  </div>
</div>
