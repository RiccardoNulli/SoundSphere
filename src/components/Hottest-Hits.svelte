<!-- Importa il componente Header -->
<script>
  import { onMount } from "svelte";
  import Header from "./Header.svelte";

  // Definizione delle variabili di stato
  let playlistTracks = [];
  let currentTrackUrl = "";

  // Esegui del codice quando il componente è montato
  onMount(async () => {
    try {
      // Imposta le credenziali dell'applicazione Spotify
      const client_id = "dd15a4511516437b92c87303851f57d2";
      const client_secret = "b290f47644df4b878603b64cfb97f7b5";

      // Codifica le credenziali client per l'autenticazione di base
      const credentials = btoa(`${client_id}:${client_secret}`);

      // Effettua una richiesta per ottenere un token di accesso utilizzando il flusso di autorizzazione di Spotify
      const tokenResponse = await fetch(
        "https://accounts.spotify.com/api/token",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/x-www-form-urlencoded",
            Authorization: `Basic ${credentials}`,
          },
          body: "grant_type=client_credentials",
        },
      );

      // Gestisci la risposta per ottenere il token di accesso
      if (!tokenResponse.ok) {
        throw new Error("Errore durante l'autenticazione con Spotify");
      }

      const tokenData = await tokenResponse.json();
      const accessToken = tokenData.access_token;

      // Ottieni le tracce della playlist utilizzando il token di accesso
      const playlistId = "37i9dQZEVXbIQnj7RRhdSX"; // ID della playlist

      const playlistResponse = await fetch(
        `https://api.spotify.com/v1/playlists/${playlistId}/tracks`,
        {
          headers: {
            Authorization: `Bearer ${accessToken}`,
          },
        },
      );

      // Gestisci la risposta per ottenere i dati della playlist
      if (!playlistResponse.ok) {
        throw new Error(
          "Errore durante il recupero delle tracce della playlist",
        );
      }

      const playlistData = await playlistResponse.json();
      playlistTracks = playlistData.items.map((item) => ({
        name: item.track.name,
        preview_url: item.track.preview_url,
        album_image: item.track.album.images[0].url, // Ottieni l'URL dell'immagine dell'album
        artist: item.track.artists.map((artist) => artist.name).join(", "), // Concatena i nomi degli artisti
        duration_ms: item.track.duration_ms, // Durata della traccia in millisecondi
      }));
    } catch (error) {
      console.error(error);
    }
  });

  // Funzione per avviare la preview della traccia
  function playPreview(trackUri) {
    currentTrackUrl = trackUri;
  }

  // Funzione per formattare la durata della traccia da millisecondi a mm:ss
  function formatDuration(duration) {
    const totalSeconds = Math.floor(duration / 1000);
    const minutes = Math.floor(totalSeconds / 60);
    const seconds = totalSeconds % 60;
    return `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
  }
</script>

<link
  href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap"
  rel="stylesheet"
/>

<!-- Includi l'header -->
<Header />

<!-- Pagina con le tracce della playlist -->
<div class="container" style="max-height: 700px;">
  <h1 class="title" style="margin-bottom: 15%; font-size: 100px;">
    Hottest Hits
  </h1>
  <div class="track-grid">
    <!-- Itera attraverso le tracce della playlist -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    {#each playlistTracks as { name, preview_url, album_image, artist, duration_ms }, index}
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <div class="track-item" on:click={() => playPreview(preview_url)}>
        <div class="track-info">
          <!-- Mostra l'immagine dell'album e le informazioni della traccia -->
          <!-- svelte-ignore a11y-img-redundant-alt -->
          <img src={album_image} alt="Album Image" class="album-image" />
          <span class="track-number">{index + 1}.</span>
          <span class="track-title">{name}</span>
          <div class="additional-info">
            <p>Artist: {artist}</p>
            <p>Duration: {formatDuration(duration_ms)}</p>
          </div>
        </div>
        <!-- Aggiungi il player audio per la preview della traccia -->
        <div class="player">
          <audio controls src={preview_url}></audio>
        </div>
      </div>
    {/each}
    <!-- Avviso di ascoltare a proprio rischio -->
    <h6 style="font-size: 1px;">Ascoltare a proprio rischio e pericolo</h6>
  </div>
</div>

<style>
  /* Stili per il container principale */
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
  }

  /* Stili per il titolo della playlist */
  .title {
    font-family: "Roboto", sans-serif;
    text-align: center;
    margin-bottom: 30px;
    color: white;
  }

  /* Stili per la griglia delle tracce */
  .track-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 40px;
  }

  /* Stili per gli elementi delle tracce */
  .track-item {
    background: linear-gradient(to right, #ee4235, #f5ab3e);
    /* Gradiente arancione chiaro -> arancione scuro */
    border-radius: 5px;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
    cursor: pointer;
  }

  .track-item:hover {
    transform: translateY(-5px);
  }

  /* Stili per le informazioni della traccia */
  .track-info {
    padding: 15px;
    text-align: center;
    color: rgb(0, 0, 0);
  }

  .track-number {
    font-weight: bold;
    margin-right: 10px;
    color: rgb(0, 0, 0);
  }

  .track-title {
    font-size: 16px;
    color: rgb(0, 0, 0);
  }

  /* Stili per il player audio */
  .player {
    background-color: linear-gradient(to right, #ee4235, #f5ab3e);
    padding: 10px;
    text-align: center;
    border-radius: 5px;
  }

  audio {
    width: 100%;
    background-color: #222; /* Riposiziona lo sfondo scuro dell'audio sotto il gradiente */
  }

  /* Stili per dispositivi di dimensioni ridotte */
  @media (max-width: 992px) {
    .track-grid {
      grid-template-columns: repeat(3, 1fr);
    }
  }

  @media (max-width: 768px) {
    .track-grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 480px) {
    .track-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
