<script>
  export let router;

  // 3rd party
  import { getClient } from '@urql/svelte';
  import { Link } from 'yrv';
  import { onMount } from 'svelte';
  import _ from 'lodash';

  // components
  import Nav from '../../components/nav/interiorNav/Top.svelte';
  import Sponsor from '../../components/SponsorSimple.svelte';
  import SessionsList from '../../components/sessions/List.svelte';
  import CardLoader from '../../components/CardLoader.svelte';

  // elements
  import StackedLayout from '../../elements/layouts/StackedLayout.svelte';
  import { ActionHeader, LinkButton } from '../../elements';

  // datasources
  import sessionsApi from '../../dataSources/api.that.tech/sessions';
  import favoritesApi from '../../dataSources/api.that.tech/favorites';

  import { thatProfile } from '../../utilities/security.js';
  import { show } from '../../store/profileNotification';

  const apiClient = getClient();

  const { querySessions } = sessionsApi(apiClient);
  $: query = querySessions();

  let createDisabled = true;

  $: if (!_.isEmpty($thatProfile)) {
    createDisabled = false;
  }

  onMount(() => {
    query = querySessions();
  });
</script>

<svelte:head>
  <title>THAT Schedule * THAT.us</title>
</svelte:head>

<StackedLayout>

  <div slot="header">
    <Nav />
    <ActionHeader title="THAT Schedule">
      {#if !createDisabled}
        <LinkButton href="/sessions/create" text="Create New Session" />
      {/if}
    </ActionHeader>
  </div>

  <div slot="body">
    <div class="text-gray-500 text-sm leading-5 text-right lowercase italic">
      <span>* Scheduled times are represented in your timezone.</span>
    </div>
    {#await querySessions()}
      <CardLoader />
    {:then sessions}
      <SessionsList {sessions} />
    {:catch error}
      <p>OH NO</p>
    {/await}
  </div>

  <div slot="footer">
    <Sponsor />
  </div>

</StackedLayout>
