<script>
</script>

<section class="footer">
    <div class="logo-row">
      <img class="logo" src="/logo-full.svg" alt="gemuchain logo">
      <!-- <div class="footer-menu-items">
        <button class="footer-menu-button">Home</button>
        <button class="footer-menu-button">Scalability</button>
        <button class="footer-menu-button">Ecosystem</button>
        <button class="footer-menu-button">Games</button>
        <button class="footer-menu-button">Power of $Gēmu</button>
        <button class="footer-menu-button">Bridge to Gēmu</button>
      </div> -->
    </div>
    <div class="social-row">
      <div class="social">
        <img class="cursor-pointer" src="/f-icons-telegram.svg" href="https://t.me/gemuchainchat" alt="twitter">
        <img class="cursor-pointer" src="/f-icons-twitter.svg" href="https://x.com/Gemuchain" alt="twitter"></div>
      <!-- <div class="legal">Privacy Policy Terms | Conditions</div> -->
    </div>
</section>

<style>
  .footer {
    box-sizing: border-box;

    position: absolute;
    bottom: 0;
    width: 100%;
    height: 154px;
    background-color: transparent;

    /* Grey/800 */
    border-top: 1px solid #464646;

    /* Inside auto layout */
    flex: none;
    order: 2;
    flex-grow: 0;

  }
  
  .logo-row {
    padding-bottom: 12px;
    padding-left: 48px;
  }

  .social-row {
    /* icons */

    /* Auto layout */
    display: flex;
    flex-direction: row;
    align-items: center;
    padding-top: 12px;
    padding-left: 48px;
    gap: 32px;

    width: 1003px;
    height: 24px;


    /* Inside auto layout */
    flex: none;
    order: 0;
    flex-grow: 1;

  }

  .cursor-pointer {
    padding-right: 24px;
  }

</style>