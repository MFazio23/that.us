<script>
  import { Footer } from '../../components';
  import CreateProfileNotification from '../../components/notifications/CreateProfile.svelte';
  import NoProfile from '../../components/notifications/NoProfile.svelte';

  import _ from 'lodash';

  import { isAuthenticated, thatProfile } from '../../utilities/security.js';

  function notifyCallback(event) {
    console.log(`Notify fired! Detail: ${event.detail}`);
  }
</script>

{#if $isAuthenticated}
  {#if _.isEmpty($thatProfile)}
    <CreateProfileNotification />
  {/if}
{/if}

<div>
  <header class="bg-gray-800 pb-32">
    <slot name="header" />
  </header>

  <main class="-mt-32">
    <div class="max-w-7xl mx-auto pb-12 px-4 sm:px-6 lg:px-8">
      <div class="bg-white rounded-lg shadow px-5 py-6 sm:px-6">
        <slot name="body" />
        <!-- default slot as fallback-->
        <slot />
      </div>
      <slot name="footer" />
    </div>
  </main>

  <NoProfile />
  <Footer />

</div>
