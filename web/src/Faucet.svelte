<script>
  import { onMount } from 'svelte';
  import { getAddress } from '@ethersproject/address';
  import { CloudflareProvider } from '@ethersproject/providers';
  import { setDefaults as setToast, toast } from 'bulma-toast';
  import Footer from './Footer.svelte';

  let input = null;
  let faucetInfo = {
    account: '0x0000000000000000000000000000000000000000',
    network: 'testnet',
    payout: 1,
    symbol: 'GEMU',
    hcaptcha_sitekey: '',
  };

  let mounted = false;
  let hcaptchaLoaded = false;

  onMount(async () => {
    const res = await fetch('/api/info');
    faucetInfo = await res.json();
    mounted = true;
  });

  window.hcaptchaOnLoad = () => {
    hcaptchaLoaded = true;
  };

  $: document.title = `${faucetInfo.symbol} ${capitalize(
    faucetInfo.network,
  )} Faucet`;

  let widgetID;
  $: if (mounted && hcaptchaLoaded) {
    widgetID = window.hcaptcha.render('hcaptcha', {
      sitekey: faucetInfo.hcaptcha_sitekey,
    });
  }

  setToast({
    position: 'bottom-center',
    dismissible: true,
    pauseOnHover: true,
    closeOnClick: false,
    animate: { in: 'fadeIn', out: 'fadeOut' },
  });

  async function handleRequest() {
    let address = input;
    if (address === null) {
      toast({ message: 'input required', type: 'is-warning' });
      return;
    }

    if (address.endsWith('.eth')) {
      try {
        const provider = new CloudflareProvider();
        address = await provider.resolveName(address);
        if (!address) {
          toast({ message: 'invalid ENS name', type: 'is-warning' });
          return;
        }
      } catch (error) {
        toast({ message: error.reason, type: 'is-warning' });
        return;
      }
    }

    try {
      address = getAddress(address);
    } catch (error) {
      toast({ message: error.reason, type: 'is-warning' });
      return;
    }

    try {
      let headers = {
        'Content-Type': 'application/json',
      };

      if (hcaptchaLoaded) {
        const { response } = await window.hcaptcha.execute(widgetID, {
          async: true,
        });
        headers['h-captcha-response'] = response;
      }

      const res = await fetch('/api/claim', {
        method: 'POST',
        headers,
        body: JSON.stringify({
          address,
        }),
      });

      let { msg } = await res.json();
      let type = res.ok ? 'is-success' : 'is-warning';
      toast({ message: msg, type });
    } catch (err) {
      console.error(err);
    }
  }

  function capitalize(str) {
    const lower = str.toLowerCase();
    return str.charAt(0).toUpperCase() + lower.slice(1);
  }
</script>

<svelte:head>
  {#if mounted && faucetInfo.hcaptcha_sitekey}
    <script
      src="https://hcaptcha.com/1/api.js?onload=hcaptchaOnLoad&render=explicit"
      async
      defer
    ></script>
  {/if}
</svelte:head>

<main>
  <section class="hero is-info is-fullheight">
    <div class="hero-head">
      <nav class="navbar">
        <div class="container">
          <div class="navbar-brand">
            <a class="navbar-item" href="../..">
              <!-- <span class="icon">
                <img src="/logo.svg" alt="Gemuchain Logo" />
              </span> -->
              <span class="icon-text"><img src="/logo-full.svg" alt="Gemuchain text" /></span>
            </a>
          </div>
        </div>
      </nav>
    </div>

    <div class="hero-body">
      <div class="container has-text-centered">
        <div class="column is-6 is-offset-3">
          <h1 class="title">
            Gēmu faucet
          </h1>
          <div class="details-form">
            <ul class="details-items">
              <li>
                <p><b>RPC </b></p>
                <a href="https://gemutest-rpc.gemuchain.io">https://gemutest-rpc.gemuchain.io</a>
              </li>
              <li>
                <p><b>Bridge </b></p>
                <a href="https://gemutest-bridge.gemuchain.io">https://gemutest-bridge.gemuchain.io</a>
              </li>
              <li>
                <p><b>Explorer </b></p>
                <a href="https://gemutest-explorer.gemuchain.io">https://gemutest-explorer.gemuchain.io</a>
              </li>
              <li>
                <p><b>ChainID </b></p>
                <p>1903648807</p>
              </li>
            </ul>
            <div class="subtitle">
              <p>
                You can claim {faucetInfo.payout} {faucetInfo.symbol} per 24 hours
              </p>
            </div>
            <div id="hcaptcha" data-size="invisible"></div>
            <div class="box">
              <div class="field is-grouped">
                <p class="control is-expanded">
                  <input
                    bind:value={input}
                    class="input"
                    type="text"
                    placeholder="Enter your address or ENS name"
                  />
                </p>
                <p class="control">
                  <button
                    on:click={handleRequest}
                    class="button is-primary"
                  >
                    Request
                  </button>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <Footer />
</main>

<style>
  .hero.is-info {
    background:
      linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)),
      url('/background.webp') no-repeat center center fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
  }
  .subtitle {
    text-align: left;
  }
  .icon {
    margin-right: 12px;
  }

  .box {
    border-radius: 19px;
    background: none;
    width: 90%;
  }

  .is-primary {
    background-color: #4228AB;
    color: #FFFFFF;
  }

  .input {
    background-color: #0D0B16;
    border-color: #4228ab;
    color: #FFFFFF;
  }
  
  ::placeholder {
    color: #989898;
    opacity: 1; /* Firefox */
  }

  ::-ms-input-placeholder { /* Edge 12 -18 */
    color: #989898;
  }

  ul {
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }

  li {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .details-form {
    /* form */

    box-sizing: border-box;

    /* Auto layout */
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 32px;
    gap: 20px;

    width: 678px;
    height: auto;

    /* Dark blue brand/900 */
    background: #0D0B16;
    /* Grey/700 */
    border: 1px solid #525252;

    /* Inside auto layout */
    flex: none;
    order: 1;
    flex-grow: 0;

  }

  .details-items {
    width: 90%;
  }

  a {
  
    text-align: left;
  }

</style>
