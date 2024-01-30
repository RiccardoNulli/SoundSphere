<script>
    import Bars from "./Bars.svelte";
    import Logo from "./Logo.svelte";
    import SideBar from "./SideBar.svelte";

// Le due funzioni aprono e chiudono la SideBar e rendono opaco il resto 
function openNav() {
  document.getElementById("mySidenav").style.width = "375px";
  setTimeout(() => 
  {
document.getElementById("opaco").style.opacity = "0.7"
document.getElementById("opaco").style.width = "100%"
document.getElementById("opaco").addEventListener("click", closeNav);
}
,100)
}
function closeNav() {
  document.getElementById("mySidenav").style.width = "0";
  document.getElementById("opaco").style.opacity = "0"
  document.getElementById("opaco").style.width = "0"
}

</script>

<!-- Tutto l'header con i suoi componenti singoli (Logo, Sidebar, SearchBar) -->

<div id="main" >
    <div class="navbar">
      
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div class="background">
        <Logo />
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div on:click= {openNav}>
            <Bars />
      
        </div>
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div class="overlay3" id="opaco" on:click={closeNav}></div>
      </div>
    
    </div>
  </div>
<div id="mySidenav" class="sidenav">
    <SideBar />
  </div>
  
<style>
      .overlay3 {
    height: 100%;
    width: 0px;
    position: fixed;
    z-index: 999;
    top: 0;
    right: 0;
    background-color: rgba(0,0,0, 0.9);
    overflow-y: auto;
    overflow-x: hidden;
    text-align: center;
    opacity: 0;
    transition: opacity 1s;
     }
    .background {
      background: linear-gradient(
        89.63deg,
        rgba(10, 9, 9, 1) 0%,
        rgba(33, 30, 30, 0.9) 100%
      );
      width: 100%;
      height: 115px;
      position: fixed;
      left: 0px;
      top: 0px;
    }
.navbar {
  overflow: hidden;
 
  position: fixed; /* Set the navbar to fixed position */
  top: 0; /* Position the navbar at the top of the page */
  width: 100%; /* Full width */
}
.sidenav {
  height: 100%; /* 100% Full-height */
  width: 0; /* 0 width - change this with JavaScript */
  position: fixed; /* Stay in place */
  z-index: 1; /* Stay on top */
  top: 0; /* Stay at the top */
  left: 0;
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 0px; /* Place content 60px from the top */
  transition: 0.5s; /* 0.5 second transition effect to slide in the sidenav */
}
/* Style page content - use this if you want to push the page content to the right when you open the side navigation */
#main {
  transition: margin-left .5s;
  padding: 20px;
}

/* On smaller screens, where height is less than 450px, change the style of the sidenav (less padding and a smaller font size) */
@media screen and (max-height: 450px) {
  .sidenav {padding-top: 15px;}
}
</style>
