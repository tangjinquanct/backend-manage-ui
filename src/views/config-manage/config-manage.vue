<template>
	<div class="layout-content">
      <div class="layout-content-main">
          <hr class="hr1" />
          <div>
            <data-config-page></data-config-page>
          </div>
          <hr class="hr1" />
          <div>
            <topology-config-page></topology-config-page>
          </div>
		  </div>

  </div>
</template>
<script>
  import dataConfigPage from './data-config/data-config.vue';

  import topologyConfigPage from './topology-config/topology-config.vue';

  export default {
      components: {dataConfigPage, topologyConfigPage},
      methods: {
          test1 () {
              alert('test1');
          },
          test2 () {
              alert('test2');
          }
      }
  }
</script>
