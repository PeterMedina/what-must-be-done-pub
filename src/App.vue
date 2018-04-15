<template>
  <div id="app">
    <div class="bg-black container mx-auto px-8 text-white">
      <PersonSelector/>
    </div>
  </div>
</template>

<script>
import '@/assets/styles/main.css';
import PersonSelector from './components/PersonSelector';

export default {
  name: 'App',
  components: {
    PersonSelector,
  },
};
</script>
