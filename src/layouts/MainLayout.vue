<template>
  <div>
  <Loader
    v-if="loading"
  />

  <div 
    v-else
    class="sb-nav-fixed"
    :class="{ 'sb-sidenav-toggled': !isOpen }"
  >
    <Navbar
      v-model="isOpen"
      @click="isOpen = !isOpen"
    />
    <div id="layoutSidenav">
      <Sidebar
        :info="info"
      />
      <div id="layoutSidenav_content">
        <main>
          <div class="container-fluid">
            <Heading>
              <h1 class="mt-4 mb-4">{{$route.name}}</h1>
            </Heading>
            <router-view/>
          </div>
        </main>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import Navbar from '@/components/app/Navbar.vue';
import Sidebar from '@/components/app/Sidebar.vue';
import Heading from '@/components/app/Heading.vue';
import Loader from '@/components/app/Loader.vue';

export default {
  name: 'mainLayout',
  data: () => ({
    isOpen: true,
    loading: true,
    info: null
  }),
  components: {
    Navbar,
    Sidebar,
    Heading,
    Loader
  },
  async mounted() {
    // Fetch info to pass it to Sidebar
    await this.$store.dispatch('fetchData')
    this.info = this.$store.getters.info
    this.loading = false
  }

}
</script>

<style>
</style>