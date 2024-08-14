<script>
  // Your existing script content here
</script>

<footer class="footer">
  <div class="footer-content">
    <div class="logo-row">
      <img class="logo" src="/logo-full.svg" alt="gemuchain logo">
    </div>
    <div class="social-row">
      <div class="social">
        <a href="https://t.me/gemuchainchat" target="_blank" rel="noopener noreferrer">
          <img class="cursor-pointer" src="/f-icons-telegram.svg" alt="telegram">
        </a>
        <a href="https://x.com/Gemuchain" target="_blank" rel="noopener noreferrer">
          <img class="cursor-pointer" src="/f-icons-twitter.svg" alt="twitter">
        </a>
      </div>
    </div>
  </div>
</footer>

<style>
  .footer {
    box-sizing: border-box;

    position: relative;
    bottom: 0;
    width: 100%;
    height: auto;
    background-color: transparent;
    margin-top: 2px;

    /* Grey/800 */
    border-top: 1px solid #464646;

    /* Inside auto layout */
    flex: none;
    order: 2;
    flex-grow: 0;

  }
  

  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .logo-row {
    margin-bottom: 20px;
  }

  .logo {
    max-width: 100%;
    height: auto;
  }

  .social-row {
    display: flex;
    align-items: center;
  }

  .social {
    display: flex;
    gap: 24px;
  }

  .cursor-pointer {
    width: 24px;
    height: 24px;
  }

  @media screen and (max-width: 768px) {
    .footer {
      padding: 15px;
      position: relative;
      bottom: 0;
    }

    .logo-row {
      margin-bottom: 15px;
    }

    .social {
      gap: 16px;
    }
  }

  @media screen and (max-width: 480px) {
    .footer {
      position: relative;
      bottom: 0;
      padding: 10px;
    }

    .logo-row {
      margin-bottom: 10px;
    }

    .social {
      gap: 12px;
    }
  }
</style>