<script>
</script>

<!-- La sezione della barra laterale con i collegamenti alla navigazione -->
<!-- Si consiglia di non modificare nulla all'interno di questa sezione -->

<div class="navigation-drawer">
  <div class="top">
    <div class="frame-34625">
      <!-- Elemento "Hottest Hits" nella barra laterale -->
      <div
        id="hottest-hits"
        class="hottest-hits"
        on:mouseover={() => {
          // Cambia il colore dello sfondo al passaggio del mouse
          document.getElementById("hottest-hits").style.backgroundColor =
            "rgba(0, 0, 0, 0.2)";
        }}
        on:mouseleave={() =>
          // Ripristina il colore dello sfondo al passaggio del mouse
          (document.getElementById("hottest-hits").style.backgroundColor = "")
        }
        on:click={() => {
          // Reindirizza l'utente alla pagina "Hottest-Hits"
          location.assign("Hottest-Hits");
        }}
      >
        <div class="left">
          <!-- Icona del fuoco -->
          <svg
            class="icon-fire"
            width="21"
            height="24"
            viewBox="0 0 21 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M6 0C9 6 0 9 0 15C0 21 6 24 6 24C3.06 18.06 12 15 12 9C12 3 6 0 6 0ZM15 9C18 15 9 18 9 24H18C19.2 24 21 22.5 21 18C21 12 15 9 15 9Z"
              fill="white"
            />
          </svg>
          <!-- Testo "Hottest Hits" -->
          <div class="hottest-hits2">
            Hottest Hits
            <br />
          </div>
        </div>
      </div>
      
      <!-- Elemento "Upcoming Albums" nella barra laterale -->
      <div
        id="upcoming-albums"
        class="upcoming-albums"
        on:mouseover={() => {
          // Cambia il colore dello sfondo al passaggio del mouse
          document.getElementById("upcoming-albums").style.backgroundColor =
            "rgba(0, 0, 0, 0.2)";
        }}
        on:mouseleave={() =>
          // Ripristina il colore dello sfondo al passaggio del mouse
          (document.getElementById("upcoming-albums").style.backgroundColor =
            "")
        }
        on:click={() => {
          // Reindirizza l'utente alla pagina "upcoming-albums"
          location.assign("upcoming-albums");
        }}
      >
        <div class="left">
          <!-- Icona del calendario -->
          <div class="icon-menu-icon">
            <svg
              class="calendar"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                d="M2 13C2 12.4477 2.44772 12 3 12H21C21.5523 12 22 12.4477 22 13V19C22 20.6569 20.6569 22 19 22H5C3.34315 22 2 20.6569 2 19V13Z"
                fill="white"
              />
              <path
                d="M7 2C7.55228 2 8 2.44772 8 3V4H16V3C16 2.44772 16.4477 2 17 2C17.5523 2 18 2.44772 18 3V4H19C20.6569 4 22 5.34315 22 7V9C22 9.55228 21.5523 10 21 10H3C2.44772 10 2 9.55228 2 9V7C2 5.34315 3.34315 4 5 4H6V3C6 2.44772 6.44772 2 7 2Z"
                fill="white"
              />
            </svg>
          </div>
          <!-- Testo "Latest Albums" -->
          <div class="upcoming-albums2">Latest Albums</div>
        </div>
      </div>
      
      <!-- Elemento "Your Favorite Albums" nella barra laterale -->
      <div
        id="your-favorite-albums"
        class="your-favorite-albums"
        on:mouseover={() => {
          // Cambia il colore dello sfondo al passaggio del mouse
          document.getElementById(
            "your-favorite-albums",
          ).style.backgroundColor = "rgba(0, 0, 0, 0.2)";
        }}
        on:mouseleave={() =>
          // Ripristina il colore dello sfondo al passaggio del mouse
          (document.getElementById(
            "your-favorite-albums",
          ).style.backgroundColor = "")
        }
        on:click={() => {
          // Reindirizza l'utente alla pagina "your-favorite-albums"
          location.assign("your-favorite-albums");
        }}
      >
        <div class="left">
          <!-- Icona della stella -->
          <svg
            class="icon-star"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M12 0L9 9H0L7.5 15L4.5 24L12 18L19.5 24L16.5 15L24 9H15L12 0Z"
              fill="white"
            />
          </svg>
          <!-- Testo "Your Favorite Albums" -->
          <div class="your-favorite-albums2">Your Favorite Albums</div>
        </div>
      </div>
      
      <!-- Elemento "Your Reviews" nella barra laterale -->
      <div
        id="your-reviews"
        class="your-reviews"
        on:mouseover={() => {
          // Cambia il colore dello sfondo al passaggio del mouse
          document.getElementById("your-reviews").style.backgroundColor =
            "rgba(0, 0, 0, 0.2)";
        }}
        on:mouseleave={() =>
          // Ripristina il colore dello sfondo al passaggio del mouse
          (document.getElementById("your-reviews").style.backgroundColor = "")
        }
        on:click={() => {
          // Reindirizza l'utente alla pagina "your-reviews"
          location.assign("your-reviews");
        }}
      >
        <div class="left">
          <!-- Icona del libro -->
          <svg
            class="icon-book"
            width="21"
            height="24"
            viewBox="0 0 21 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M3 0C2.79 0 2.61 0.03 2.43 0.09C1.26 0.33 0.33 1.26 0.09 2.43C0 2.61 0 2.79 0 3V19.5C0 21.99 2.01 24 4.5 24H21V21H4.5C3.66 21 3 20.34 3 19.5C3 18.66 3.66 18 4.5 18H21V1.5C21 0.66 20.34 0 19.5 0H18V9L15 6L12 9V0H3Z"
              fill="white"
            />
          </svg>
          <!-- Testo "Your Reviews" -->
          <div class="your-reviews2">Your Reviews</div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- Stili CSS per la barra laterale -->
<style>
  /* Stili per la barra laterale e i suoi elementi */
  .navigation-drawer,
  .navigation-drawer * {
    box-sizing: border-box;
  }
  .navigation-drawer {
    /* Stili di sfondo e dimensioni */
    background: linear-gradient(
      172.6deg,
      rgba(0, 0, 0, 1) 0%,
      rgba(134, 1, 1, 1) 0.009999999747378752%,
      rgba(208, 38, 0, 1) 52.49999761581421%,
      rgba(209, 100, 0, 1) 100%
    );
    padding: 24px;
    display: flex;
    flex-direction: column;
    gap: 141px;
    align-items: flex-start;
    justify-content: flex-start;
    width: 100%;
    height: 100%;
    position: relative;
  }
  .top {
    /* Stili per la sezione superiore */
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    align-self: stretch;
    flex-shrink: 0;
    height: 525px; /* Altezza della sezione superiore */
    position: relative;
  }
  .frame-34625 {
    /* Stili per il frame principale */
    display: flex;
    flex-direction: column;
    gap: 8px;
    align-items: flex-start;
    justify-content: flex-start;
    align-self: stretch;
    flex-shrink: 0;
    position: relative;
  }
  /* Stili per ciascun elemento della barra laterale */
  .hottest-hits,
  .upcoming-albums,
  .your-favorite-albums,
  .your-reviews {
    border-radius: 8px;
    padding: 12px;
    display: flex;
    flex-direction: row;
    gap: 12px;
    align-items: center;
    justify-content: flex-start;
    align-self: stretch;
    flex-shrink: 0;
    position: relative;
  }
  /* Stili per l'icona e il testo di ciascun elemento */
  .left {
    display: flex;
    flex-direction: row;
    gap: 6px;
    align-items: center;
    justify-content: flex-start;
    flex: 1;
    position: relative;
  }
  /* Stili per le icone */
  .icon-fire,
  .calendar,
  .icon-star,
  .icon-book {
    flex-shrink: 0;
    position: relative;
    overflow: visible;
  }
  /* Stili per il testo */
  .hottest-hits2,
  .upcoming-albums2,
  .your-favorite-albums2,
  .your-reviews2 {
    color: #ffffff; /* Colore del testo */
    text-align: left;
    font-family: "Inter-SemiBold", sans-serif;
    font-size: 15px;
    font-weight: 600;
    position: relative;
  }
</style>
