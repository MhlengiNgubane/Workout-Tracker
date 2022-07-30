<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status Message -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md shadow-lg"
    >
      <p class="text-at-light-green">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Create -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <!-- Form -->
      <form @submit.prevent="createWorkout" class="flex flex-col gap-y-5 w-full">
        <h1 class="text-2xl text-red-400">Add Exercise</h1>

        <!-- Workout Name -->
        <div class="flex flex-col">
          <label for="workout-name" class="mb-1 text-sm text-red-400"
            >Exercise Name</label
          >
          <input
            type="text"
            required
            class="p-2 text-gray-500 focus:outline-none"
            id="workout-name" 
            v-model="exerciseName"
          />
        </div>

        <!-- Workout Type -->
        <div class="flex flex-col">
          <label for="workout-type" class="mb-1 text-sm text-red-400"
            >Exercise Type</label
          >
          <select
            id="workout-type"
            class="p-2 text-gray-500 focus:outline-none"
            required
            @change="workoutChange"
            v-model="exerciseType"
          >
            <option value="select-exercise">Select Workout</option>
            <option value="strength">Strength Training</option>
            <option value="cardio">Cardio</option>
            <option value="sports">Sports</option>
            <option value="cycling">Cycling</option>
          </select>
        </div>

        <!-- Strength Training Inputs -->
        <div v-if="exerciseType === 'strength'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm text-red-400"
                >Exercise
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="sets" class="mb-1 text-sm text-red-400">Sets</label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="reps" class="mb-1 text-sm text-red-400">Reps </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="weight" class="mb-1 text-sm text-red-400"
                >Weight (Kg's)
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/bin.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-start text-sm
            text-white bg-red-400 duration-200 border-solid
            border-2 border-transparent hover:border-red-400 hover:bg-white
            hover:text-red-400"
          >
            Add Workout
          </button>
        </div>

        <!-- Cardio Inputs -->
        <div v-if="exerciseType === 'cardio'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm text-red-400"
                >Type
              </label>
              <select
                id="cardio-type"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.cardioType"
              >
                <option value="#">Select Type</option>
                <option value="run">Runs</option>
                <option value="walk">Walk</option>
              </select>
            </div>
            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm text-red-400"
                >Distance
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm text-red-400"
                >Duration
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="pace" class="mb-1 text-sm text-red-400">Pace </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/bin.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-start text-sm
            text-white bg-red-400 duration-200 border-solid
            border-2 border-transparent hover:border-red-400 hover:bg-white
            hover:text-red-400"
          >
            Add Workout
          </button>
        </div>

        <!-- Sports Inputs -->
        <div v-if="exerciseType === 'sports'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="sports-type" class="mb-1 text-sm text-red-400"
                >Type
              </label>
              <select
                id="sports-type"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sportsType"
              >
                <option value="#">Select Type</option>
                <option value="Aerobics">Aerobics</option>
                <option value="Aerobics(Intensive)">Aerobics (Intensive)</option>
                <option value="Badminton">Badminton</option>
                <option value="Badminton(Match)">Badminton (Match)</option>
                <option value="Ballet">Ballet</option>
                <option value="Baseball">Baseball</option>
                <option value="Baseball(Game)">Baseball (Game)</option>
                <option value="Basketball">Basketball</option>
                <option value="Basketball(Game)">Basketball (Game)</option>
                <option value="Beach vollyball">Beach vollyball</option>
                <option value="Billiards">Billiards</option>
                <option value="Bowling">Bowling</option>
                <option value="Boxing(Match)">Boxing (Match)</option>
                <option value="Boxing(Sparring)">Boxing (Sparring)</option>
                <option value="Skydiving">Skydiving</option>
                <option value="Football">Football </option>
                <option value="Football(Match)">Football (Match))</option>
                <option value="Squash">Squash</option>
                <option value="Swimming">Swimming</option>
              </select>
            </div>
            <div class="flex flex-col flex-1">
              <label for="time" class="mb-1 text-sm text-red-400"
                >Start Time
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.time"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm text-red-400"
                >Duration
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="goal" class="mb-1 text-sm text-red-400">Goal </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.goal"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/bin.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-start text-sm
            text-white bg-red-400 duration-200 border-solid
            border-2 border-transparent hover:border-red-400 hover:bg-white
            hover:text-red-400"
          >
            Add Workout
          </button>
        </div>

        <!-- Cycling Inputs -->
        <div v-if="exerciseType === 'cycling'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="cycling-type" class="mb-1 text-sm text-red-400"
                >Type
              </label>
              <select
                id="cardio-type"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.cyclingType"
              >
                <option value="#">Select Type</option>
                <option value="Cycling(100W Light)">Cycling (100W Light)</option>
                <option value="Cycling(150W Moderate)">Cycling (150W Moderate)</option>
                <option value="Cycling(200W Vigorious)">Cycling (200W Vigorious)</option>
              </select>
            </div>
            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm text-red-400"
                >Distance
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm text-red-400"
                >Duration
              </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="pace" class="mb-1 text-sm text-red-400">Pace </label>
              <input
                required
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/bin.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-sm self-start text-sm
            text-white bg-red-400 duration-200 border-solid
            border-2 border-transparent hover:border-red-400 hover:bg-white
            hover:text-red-400"
          >
            Add Workout
          </button>
        </div>

        <button
          type="submit"
          class="mt-6 py-2 px-6 rounded-sm self-start text-sm
          text-white bg-red-400 duration-200 border-solid
          border-2 border-transparent hover:border-red-400 hover:bg-white
          hover:text-red-400"
        >
          Record Exercise
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from "../supabase/supabase";
export default {
  name: "create",
  setup() {
    // Create data
    const exerciseName = ref("");
    const exerciseType = ref("select-exercise");
    const exercises = ref([]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);

    // Add exercise
    const addExercise = () => {
      if (exerciseType.value === "strength") {
        exercises.value.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      } else if (exerciseType.value === "cardio") {
      exercises.value.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      });
        return;
      } else if (exerciseType.value === "cycling") {
      exercises.value.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      });
      return;
      } else  
      exercises.value.push({
        id: uid(),
        sportsType: "",
        time: "",
        duration: "",
        goal: "",
      });
    
    };
    
  
    // Delete exercise
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter((exercise) => exercise.id !== id);
        return;
      }
      errorMsg.value = "Cannot remove, need to at least have one workout";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // Listens for chaging of workout type input
    const workoutChange = () => {
      exercises.value = [];
      addExercise();
    };

    // Create workout
    const createWorkout = async () => {
      try {
        const { error } = await supabase.from("workouts").insert([
          {
            exerciseName: exerciseName.value,
            exerciseType: exerciseType.value,
            exercises: exercises.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Workout Created!";
        exerciseName.value = null;
        exerciseType.value = "select-exercise";
        exercises.value = [];
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value = `${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return {
      exerciseName,
      exerciseType,
      exercises,
      statusMsg,
      errorMsg,
      addExercise,
      workoutChange,
      deleteExercise,
      createWorkout,
    };
  },
};
</script>
