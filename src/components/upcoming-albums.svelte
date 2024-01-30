<script>
  import { onMount } from "svelte";

  import Header from "./Header.svelte";

  // Costanti e variabili di stato
  const client_id = "dd15a4511516437b92c87303851f57d2";
  const client_secret = "b290f47644df4b878603b64cfb97f7b5";
  let newReleases = [];
  let albumTracks = [];
  const albumIDs = [];
  let selectedAlbum = null;
  let showPopup = false;
  let reviewText = "";
  let showReviewOverlay = false;

  // Funzione per ottenere il token di accesso
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

  // Gestione del clic su un album
  function handleAlbumClick(album) {
    selectedAlbum = album;
    getToken().then((response) => {
      getAlbumTracks(response.access_token, album.id).then((tracks) => {
        albumTracks = tracks.items;
      });
    });
  }

  // Inizializzazione della pagina
  async function initialize() {
    const tokenResponse = await getToken();
    const newReleasesResponse = await getNewReleases(
      tokenResponse.access_token,
    );

    // Ottieni gli ID degli album dai nuovi rilasci
    for (let i = 0; i < newReleasesResponse.albums.total; i++) {
      const albumId = newReleasesResponse.albums.items[i]?.id;
      if (albumId) {
        albumIDs[i] = albumId;
      }
    }

    newReleases = newReleasesResponse.albums.items;
  }

  onMount(initialize);

  // Gestione del clic su "Aggiungi recensione"
  function handleAddReviewClick() {
    showReviewOverlay = true;
  }

  // Gestione del clic su "Preferito"
  function handleFavouriteClick(album) {
    localStorage.setItem(album.name + "_album", JSON.stringify(album));
    showPopup = true;
    setTimeout(() => {
      showPopup = false;
    }, 2000); // Nascondi il popup dopo 2 secondi
  }

  // Salvataggio della recensione
  function saveReview() {
    if (reviewText.trim() !== "") {
      const review = {
        name: selectedAlbum.name,
        review: reviewText.trim(),
      };
      localStorage.setItem(
        selectedAlbum.name + "_review",
        JSON.stringify(review),
      );
      showReviewOverlay = false;
      reviewText = "";
    }
  }
</script>

<!-- Includi l'header -->
<Header />
<h1 style="margin-bottom: 15%; font-size: 100px;">Latest Albums</h1>

<div class="album-container" style="max-height: 700px;">
  <!-- Itera attraverso i nuovi rilasci -->
  {#each newReleases as release}
    <!-- Gestisci il clic su un album -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="album-box" on:click={() => handleAlbumClick(release)}>
      {#if release.images.length > 0}
        <img src={release.images[0].url} alt={release.name} />
      {/if}
      <div class="album-info">
        <p style="font-weight: bold;">{release.name}</p>
        <p>Artist: {release.artists[0].name}</p>
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
            <h2 style="font-weight: bold;">Album Tracks:</h2>
            <ul>
              <!-- Itera attraverso le tracce dell'album -->
              {#each albumTracks as track}
                <li>{track.name}</li>
              {/each}
            </ul>
          </div>
        </div>
        <div class="album-meta">
          <p style="font-weight: bold;">{selectedAlbum.name}</p>
          <p>Artist: {selectedAlbum.artists[0].name}</p>
          <!-- Pulsanti per chiudere, aggiungere ai preferiti e aggiungere una recensione -->
          <button
            style="font-weight: bold;"
            on:click={() => (selectedAlbum = null)}>Close</button
          >
          <button
            style="font-weight: bold;"
            on:click={() => handleFavouriteClick(selectedAlbum)}
            >Favourite</button
          >
          <button style="font-weight: bold;" on:click={handleAddReviewClick}
            >Add Review</button
          >
        </div>
      </div>
    </div>
  </div>
{/if}

<!-- Popup per confermare l'aggiunta ai preferiti -->
{#if showPopup}
  <div class="popup">Added to Favorite!</div>
{/if}

<!-- Overlay per aggiungere una recensione -->
{#if showReviewOverlay}
  <div class="overlay">
    <div class="overlay-content">
      <textarea
        style="width: 400px;"
        bind:value={reviewText}
        rows="6"
        placeholder="Write your review..."
      ></textarea>
      <div class="album-meta buttons">
        <button style="font-weight: bold;" on:click={saveReview}
          >Save Review</button
        >
        <button
          style="font-weight: bold;"
          on:click={() => (showReviewOverlay = false)}>Cancel</button
        >
      </div>
    </div>
  </div>
{/if}

<style>
  /* Stili per il contenitore degli album */
  .album-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    display: grid;
    grid-template-columns: repeat(
      3,
      1fr
    ); /* Organizza le recensioni in una griglia responsiva */
    gap: 60px; /* Distribuisce uniformemente gli elementi nelle colonne */
  }

  /* Stili per i box degli album */
  .album-box {
    box-sizing: border-box;
    background: linear-gradient(to right, #ee4235, #f5ab3e);
    border-radius: 2%;
    padding: 10px;
    margin: 10px;
    width: 100%; /* Imposta la larghezza dell'elemento a meno del 50% per evitare problemi di overflow */
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

  /* Stili per il popup */
  .popup {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background-color: rgba(0, 0, 0, 0.8);
    color: white;
    padding: 10px 20px;
    border-radius: 5px;
    z-index: 9999;
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
    text-align: center;
  }

  /* Stili per i pulsanti */
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
