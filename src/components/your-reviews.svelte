<script>
  // Importa il componente Header da Header.svelte
  import Header from "./Header.svelte";

  // Funzione per ottenere tutte le recensioni salvate nello storage locale
  function allStorage() {
    var values = [],
      keys = Object.keys(localStorage),
      i = keys.length;

    // Itera attraverso tutte le chiavi dello storage locale
    while (i--) {
      // Verifica se la chiave include "_review"
      if (keys[i].includes("_review")) {
        // Se sì, aggiunge il valore associato alla chiave all'array "values"
        values.push(localStorage.getItem(keys[i]));
      }
    }

    return values;
  }

  // Ottiene tutte le recensioni e le converte da JSON a oggetti JavaScript
  let reviews = allStorage().map((element) => JSON.parse(element));

  // Funzione per eliminare una recensione
  function deleteReview(review) {
    // Rimuove la recensione dallo storage locale
    localStorage.removeItem(review.name + "_review");
    // Aggiorna l'array di recensioni rimuovendo quella eliminata
    reviews = reviews.filter((r) => r !== review);
  }
</script>

<!-- Titolo della pagina -->
<h1 style="margin-bottom: 15%; font-size: 100px;">Your Reviews</h1>
<div class="home">
  <!-- Includi l'header -->
  <Header />
  <div class="album-container" style="max-height: 700px;">
    <!-- Mostra le recensioni in una griglia -->
    <div class="reviews-grid">
      <!-- Itera attraverso tutte le recensioni -->
      {#each reviews as review}
        <div class="review-box">
          <!-- Mostra il nome dell'album e la recensione -->
          <p><strong>{review.name}</strong></p>
          <p>{review.review}</p>
          <!-- Aggiungi il pulsante "Delete" per eliminare la recensione -->
          <button class="delete-button" on:click={() => deleteReview(review)}
            >Delete</button
          >
        </div>
      {/each}
    </div>
  </div>
</div>

<!-- Stili CSS -->
<style>
  /* Stili per il contenitore degli album */
  .album-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between; /* Distribuisce uniformemente gli elementi nelle colonne */
  }

  /* Stili per la griglia delle recensioni */
  .reviews-grid {
    display: grid;
    grid-template-columns: repeat(
      5,
      1fr
    ); /* Organizza le recensioni in una griglia responsiva */
    gap: 40px;
  }

  /* Stili per il box di ogni recensione */
  .review-box {
    background: linear-gradient(to right, #ee4235, #f5ab3e);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

  /* Stili per il pulsante "Delete" */
  .delete-button {
    margin-top: 10px;
    padding: 8px 16px;
    background-color: #000000;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .delete-button:hover {
    background-color: #e2463f;
  }
</style>
