<template>
  <div>
    <div :class="{'hidden': hasStarted === true}">
      <div class="flex items-center justify-center h-screen">
        <div>
          <h1 class="font-normal leading-none text-5xl">
            What must be done?
          </h1>
          <input
            class="bg-grey-darkest inline-block mt-4 mb-6 p-4 text-4xl text-white uppercase w-full"
            placeholder="Take out the trash"
            type="text"
            v-model="task">
          <div v-show="!task">
            <p class="text-4xl">
              Enter a task above
            </p>
          </div>
          <div v-show="task">
            <button @click="startSelection" class="text-4xl text-white">
              Start Game
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-show="hasStarted && hasFinished === false" class="h-screen">
      <div class="py-8">
        <div>
          <h1 class="font-normal leading-none mb-4 text-5xl">
            Who will {{ task }}?
          </h1>
          <div class="flex flex-wrap -mx-4">
            <!-- eslint-disable-next-line -->
            <div class="mb-4 md:mb-0 px-4 w-1/3 md:w-1/6" v-for="person in people" :key="person.name">
              <!-- eslint-disable-next-line -->
              <img class="border-4" :class="{'border-orange': selectedPerson === person.name}" :src="person.photo" :alt="person.name">
              <h2 class="font-normal mt-3 text-center">
                {{ person.name }}
              </h2>
            </div>
          </div>
        </div>
        <div v-show="safe.length > 0">
          <h2 class="font-normal leading-none mt-8 mb-4 text-5xl">
            Not It
          </h2>
          <div class="flex flex-wrap -mx-4 md:w-1/2">
            <div class="mb-4 md:mb-0 px-4 w-1/3 md:w-1/6" v-for="saved in safe" :key="saved.name">
              <div class="border-2 border-white">
                <img class="block greyscale" :src="saved.photo" :alt="saved.name">
              </div>
              <h2 class="font-normal mt-3 text-lg text-center">
                {{ saved.name }}
              </h2>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div :class="{'hidden': hasFinished === false}" class="h-screen">
      <div class="py-8">
        <h2 class="font-normal leading-none mb-4 text-5xl">
          {{ people[0].name }} will {{ task }}!
        </h2>
        <div class="md:flex md:items-center md:-mx-4">
          <div class="md:px-4 w-full md:w-1/4">
            <!-- eslint-disable-next-line -->
            <img class="border-4 border-whte mb-4 md:mb-0" :src="people[0].photo" :alt="people[0].name">
          </div>
          <div class="md:px-4 w-full md:w-3/4">
            <div class="flex">
              <div class="md:ml-8 text-4xl md:text-6xl">&ldquo;</div>
              <p class="text-4xl md:text-6xl">
                {{ people[0].quote }}&rdquo;
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import people from '../api/people.json';

export default {
  name: 'PersonSelector',
  data() {
    return {
      hasStarted: false,
      hasFinished: false,
      pass: 0,
      people,
      safe: [],
      selectedPerson: '',
      task: '',
    };
  },
  methods: {
    makeSelection() {
      if (this.people.length > 1) {
        const randomNumber = Math.floor(Math.random() * this.people.length);
        const currentSelection = this.people[randomNumber];
        const index = this.people.indexOf(currentSelection);

        this.pass += 1;
        this.selectedPerson = currentSelection.name;
        this.selectedIndex = index;

        if ((this.pass % 50) === 0) {
          this.safe.push(currentSelection);
          this.people.splice(this.selectedIndex, 1);
        }
      }

      if (this.people.length === 1) {
        this.hasFinished = true;
      }
    },
    startSelection() {
      this.hasStarted = true;
      const newThis = this;
      // eslint-disable-next-line
      return setInterval(function () {
        newThis.makeSelection();
      }, 100);
    },
  },
};
</script>
