<!-- Details.svelte -->

<script>
  import { onMount } from 'svelte';

  export let animeData;

  let animeId;

  onMount(() => {
    // Ottieni animeId da localStorage
    animeId = localStorage.getItem('selectedAnimeId');

    // Assicurati che animeId sia una stringa (o altra logica di gestione)
    if (!animeId) {
      console.error('L\'animeId non è presente in localStorage o è vuoto.');
      return;
    }

    // Esegui la richiesta solo se animeId è valido
    fetchAnimeData();
  });

  async function fetchAnimeData() {
    try {
      const response = await fetch(`https://api.jikan.moe/v4/anime/${animeId}/full`);
      const data = await response.json();
      animeData = data;
    } catch (error) {
      console.error('Errore durante la richiesta dei dati dell\'anime:', error);
    }
  }

</script>

{#if animeData}
  <div class="container items-center display mx-auto">

    <div class="title card wrapper">
      <h1 class="font-bold text-6xl">{animeData.data.title}</h1>
    </div>
      <br>
    <div class="wrapper card">
      <div class="image card">
        <img src="{animeData.data.images.jpg.image_url}"> 
      </div>

      <div class="details mx-auto">
        <h1>Titolo originale: {animeData.data.title_japanese}</h1>
        <p>Durata: {animeData.data.duration}</p>
        <p>Episodi: {animeData.data.episodes}</p>

        <p>Popolarità: {animeData.data.popularity}</p>
        <p>Rank: {animeData.data.rank}</p>

        <p>Anno: {animeData.data.year}</p>
        <p>Streaming: {animeData.data.streaming[0].url}</p>
      </div>
    </div>

      <br>
    <div class="wrapper card">
      <div class="container trailer mx-auto card">
        <iframe src="{animeData.data.trailer.embed_url}" frameborder="0" width="100%" height="1100%"></iframe>
      </div>
      <div class="syn mx-auto" width="50">
        <p> {animeData.data.synopsis}</p>
      </div>
    </div>
  </div>


  
  <!----<div id="anime-details">
  </div>
-->
{/if}
