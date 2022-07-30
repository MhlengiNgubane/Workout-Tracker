<template>
  <div v-if="dataLoaded" class="container mt-10 px-4">
    <!-- No Data -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h1 class="text-2xl">Looks empty here...</h1>
      <router-link
        class="mt-6 py-2 px-6 rounded-sm  text-sm
      text-white bg-red-400 duration-200 border-solid
      border-2 border-transparent hover:border-red-400 hover:bg-white
      hover:text-red-400"
        :to="{ name: 'Create' }"
        >Create Workout</router-link
      >
    </div>

    <!-- Data -->
    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <router-link
        class="flex flex-col items-center bg-light-grey p-8 shadow-md cursor-pointer"
        :to="{ name: 'View-Workout', params: { workoutId: workout.id } }"
        v-for="(workout, index) in data"
        :key="index"
      >
        
        
        <img
          v-if="workout.exerciseType === 'strength'"
          class="h-24 w-auto"
          src="@/assets/images/dumbbell.png"
          alt=""
        />

        <img
          v-else-if="workout.exerciseType === 'cardio'"
          class="h-24 w-auto"
          src="@/assets/images/running.png"
          alt=""
        />

        <img
          v-else-if="workout.exerciseType === 'sports'"
          class="h-24 w-auto"
          src="@/assets/images/sports.png"
          alt=""
        />

        <img
          v-else
          class="h-24 w-auto"
          src="@/assets/images/cycling.png"
          alt=""
        />

        <p
          class="mt-6 py-1 px-3 text-xs text-white bg-red-400 shadow-md rounded-lg"
        >
          {{ workout.exerciseType }}
        </p>

        <h1 class="mt-8 mb-2 text-center text-xl text-red-400">
          {{ workout.exerciseName }}
        </h1>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/supabase";
export default {
  name: "home",
  components: {},
  setup() {
    // Create data / vars
    const data = ref([]);
    const dataLoaded = ref(null);

    // Get data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase.from("workouts").select("*");
        if (error) throw error;
        data.value = workouts;
        dataLoaded.value = true;
      } catch (error) {
        console.warn(error.message);
      }
    };

    // Run data function
    getData();

    return { data, dataLoaded };
  },
};
</script>
