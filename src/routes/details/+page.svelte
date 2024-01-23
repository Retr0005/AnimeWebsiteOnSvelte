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
  <div class="display">
    <h1 class="h1 font-sans text-7xl font-medium justify-center items-center mx-auto text-center my-8 ">{animeData.data.title_english}</h1>
    <embed class="mx-auto h-full w-4/5 my-8" type="video/webm" src="{animeData.data.trailer.embed_url}">
      <p> {animeData.data.synopsis}</p>
  </div>
  

  
  <!----<div id="anime-details">
    <h1>{animeData.data.title_japanese}</h1>
    <p>Durata: {animeData.data.duration}</p>
    <p>Episodi: {animeData.data.episodes}</p>
  
    <p>Popolarità: {animeData.data.popularity}</p>
    <p>Rank: {animeData.data.rank}</p>
    
    <p>Anno: {animeData.data.year}</p>
    <p>Streaming: {animeData.data.streaming[0].url}</p>
  </div>
-->
{/if}
