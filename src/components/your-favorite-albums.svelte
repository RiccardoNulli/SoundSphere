<script>
  // Importa la funzione onMount da Svelte
  import { onMount } from "svelte";

  // Importa il componente Header da Header.svelte
  import Header from "./Header.svelte";

  // Costanti e variabili di stato
  const client_id = "dd15a4511516437b92c87303851f57d2";
  const client_secret = "b290f47644df4b878603b64cfb97f7b5";
  let newReleases = [];
  let albumTracks = [];
  const albumIDs = [];
  let selectedAlbum = null;

  // Funzione per ottenere il token di accesso da Spotify
  async function getToken() {
    const response = await fetch("https://accounts.spotify.com/api/token", {
      method: "POST",
      body: new URLSearchParams({
        grant_type: "client_credentials",
      }),
      headers: {
        "Content-Type": "application/x-www-form-urlencoded",
        Authorization: "Basic " + btoa(client_id + ":" + client_secret),
      },
    });

    return await response.json();
  }

  // Funzione per ottenere i nuovi rilasci da Spotify
  async function getNewReleases(access_token) {
    const response = await fetch(
      "https://api.spotify.com/v1/browse/new-releases",
      {
        method: "GET",
        headers: { Authorization: "Bearer " + access_token },
      },
    );

    return await response.json();
  }

  // Funzione per ottenere le tracce di un album da Spotify
  async function getAlbumTracks(access_token, albumId) {
    const response = await fetch(
      `https://api.spotify.com/v1/albums/${albumId}/tracks`,
      {
        method: "GET",
        headers: { Authorization: "Bearer " + access_token },
      },
    );

    return await response.json();
  }

  // Gestisce il clic su un album
  function handleAlbumClick(album) {
    selectedAlbum = album;
    getToken().then((response) => {
      getAlbumTracks(response.access_token, album.id).then((tracks) => {
        albumTracks = tracks.items;
      });
    });
  }

  // Inizializza la pagina
  async function initialize() {
    const tokenResponse = await getToken();
    const newReleasesResponse = await getNewReleases(
      tokenResponse.access_token,
    );

    // Ottiene gli ID degli album dai nuovi rilasci
    for (let i = 0; i < newReleasesResponse.albums.total; i++) {
      const albumId = newReleasesResponse.albums.items[i]?.id;
      if (albumId) {
        albumIDs[i] = albumId;
      }
    }

    // Salva i nuovi rilasci nella variabile di stato
    newReleases = newReleasesResponse.albums.items;
  }

  // Esegui l'inizializzazione al montaggio del componente
  onMount(initialize);

  // Gestisce il clic sul pulsante "Remove from Favourites"
  function handleFavouriteClick(key) {
    // Rimuove l'elemento dai preferiti e ricarica la pagina
    localStorage.removeItem(key);
    location.reload();
  }

  // Ottiene tutti gli album salvati nei preferiti
  function allStorage() {
    var values = [],
      keys = Object.keys(localStorage),
      i = keys.length;

    while (i--) {
      if (keys[i].includes("_album")) {
        values.push(localStorage.getItem(keys[i]));
      }
    }

    return values;
  }

  // Ottiene e converte gli album salvati nei preferiti
  let album = allStorage();
  album = album.map((element) => JSON.parse(element));
</script>

<div class="home">
  <!-- Titolo della pagina -->
  <h1 style="margin-bottom: 15%; font-size: 100px;">Your Favorite Albums</h1>
  <!-- Includi l'header -->
  <Header />
  <div class="album-container" style="max-height: 700px;">
    <!-- Itera attraverso gli album salvati nei preferiti -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    {#each album as release}
      <!-- Box per visualizzare un album -->
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <div class="album-box" on:click={() => handleAlbumClick(release)}>
        <img src={release.images[0].url} alt={release.name} />

        <div class="album-info">
          <!-- Mostra il nome dell'album e l'artista -->
          <p>Nome dell'album: {release.name}</p>
          <p>Artista: {release.artists[0].name}</p>
        </div>
      </div>
    {/each}
  </div>

  <!-- Overlay per visualizzare dettagli dell'album selezionato -->
  {#if selectedAlbum}
    <div class="overlay">
      <div class="overlay-content">
        <div class="album-info">
          <div class="overlay-details">
            <div class="overlay-image">
              <img src={selectedAlbum.images[0].url} alt={selectedAlbum.name} />
            </div>
            <div class="overlay-tracks">
              <h2>Tracce dell'album:</h2>
              <ul>
                <!-- Itera attraverso le tracce dell'album -->
                {#each albumTracks as track}
                  <li>{track.name}</li>
                {/each}
              </ul>
            </div>
          </div>
          <div class="album-meta">
            <!-- Mostra il nome dell'album e l'artista -->
            <p>Nome dell'album: {selectedAlbum.name}</p>
            <p>Artista: {selectedAlbum.artists[0].name}</p>
            <!-- Pulsanti per chiudere e rimuovere dai preferiti -->
            <button on:click={() => (selectedAlbum = null)}>Close</button>
            <button
              on:click={() =>
                handleFavouriteClick(selectedAlbum.name + "_album")}
              >Remove from Favourites</button
            >
          </div>
        </div>
      </div>
    </div>
  {/if}
  <!-- Fine della home -->
</div>

<!-- Stili CSS -->
<style>
  /* Stili per il contenitore degli album */
  .album-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between; /* Distribuisce uniformemente gli elementi nelle colonne */
  }

  /* Stili per il box degli album */
  .album-box {
    box-sizing: border-box;
    background: linear-gradient(to right, #ee4235, #f5ab3e);
    border-radius: 2%;
    padding: 10px;
    margin: 10px;
    width: 48%; /* Imposta la larghezza dell'elemento a meno del 50% per evitare problemi di overflow */
    display: flex;
    flex-direction: column;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Aggiungi l'ombra */
  }

  /* Stili per le immagini degli album */
  .album-box img {
    width: 100%; /* L'immagine occupa tutta la larghezza del contenitore */
    height: auto;
    border-radius: 2%;
    margin-bottom: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Aggiungi l'ombra */
  }

  .album-info {
    flex-grow: 1;
  }

  @media (max-width: 600px) {
    .album-box {
      width: 100%; /* A una singola colonna per schermi più piccoli */
    }
  }

  /* Stili per l'overlay */
  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .overlay-content {
    background: #1d1d1d;
    padding: 20px;
    border-radius: 5px;
    text-align: center;
  }

  .overlay-tracks {
    background: #1d1d1d;
    padding: 20px;
    border-radius: 5px;
    text-align: right;
    color: rgb(255, 255, 255);
  }

  .overlay img {
    width: 50%;
    height: auto;
    margin-bottom: 10px;
  }

  .overlay-details {
    display: flex;
    align-items: center;
  }

  .overlay-image {
    flex: 0 0 50%;
  }

  .overlay-image img {
    width: 100%;
    height: auto;
  }

  .overlay-tracks {
    flex: 1;
    text-align: left;
  }

  /* Stili per il meta dell'album */
  .album-meta {
    text-align: center;
  }
  .album-meta button {
    padding: 10px 20px;
    margin: 5px;
    background: linear-gradient(to right, #ee4235, #f5ab3e); /* Verde */
    color: rgb(0, 0, 0);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
</style>
