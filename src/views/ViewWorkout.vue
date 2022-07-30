<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- App Msg -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 rounded-md shadow-md bg-light-grey"
    >
      <p class="text-at-light-green">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">
        {{ errorMsg }}
      </p>
    </div>

    <div v-if="dataLoaded">
      <!-- General Workout Info -->
      <div
        class="flex flex-col items-center p-8 rounded-md shadow-md 
      bg-light-grey relative"
      >
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer
            bg-red-400 shadow-lg"
            @click="editMode"
          >
            <img class="h-3.5 w-auto" src="@/assets/images/pencil-light.png" alt="" />
          </div>
          <div
            @click="deleteWorkout"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer
            bg-red-400 shadow-lg"
          >
            <img class="h-3.5 w-auto" src="@/assets/images/trash-light.png" alt="" />
          </div>
        </div>

        <img
          v-if="data.exerciseType === 'strength'"
          class="h-24 w-auto"
          src="@/assets/images/dumbbell.png"
          alt=""
        />

        <img
          v-if="data.exerciseType === 'cardio'"
          class="h-24 w-auto"
          src="@/assets/images/running.png"
          alt=""
        />

        <img
          v-if="data.exerciseType === 'sports'"
          class="h-24 w-auto"
          src="@/assets/images/sports.png"
          alt=""
        />

        <img
          v-if="data.exerciseType === 'cycling'"
          class="h-24 w-auto"
          src="@/assets/images/cycling.png"
          alt=""
        />

        <span
          class="mt-6 py-1.5 px-5 text-xs text-white bg-red-400
        rounded-lg shadow-md"
        >
          {{ data.exerciseType }}
        </span>

        <div class="w-full mt-6">
          <input
            v-if="edit"
            type="text"
            class="p-2 w-full text-gray-500 focus:outline-none"
            v-model="data.exerciseName"
          />
          <h1 v-else class="text-red-400 text-2xl text-center">
            {{ data.exerciseName }}
          </h1>
        </div>
      </div>

      <!-- Exercises -->
      <div
        class="mt-10 p-8 rounded-md flex flex-col item-center
      bg-light-grey shadow-md"
      >
        <!-- Strength Training -->
        <div v-if="data.exerciseType === 'strength'" class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="workout-name" class="mb-1 text-sm text-red-400">
                Exercise
              </label>
              <input
                id="workout-name"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.workout"
              />
              <p v-else>{{ item.workout }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="sets" class="mb-1 text-sm text-red-400">
                Sets
              </label>
              <input
                v-if="edit"
                id="sets"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.sets"
              />
              <p v-else>{{ item.sets }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="reps" class="mb-1 text-sm text-red-400">
                Reps
              </label>
              <input
                v-if="edit"
                id="reps"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.reps"
              />
              <p v-else>{{ item.reps }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="weight" class="mb-1 text-sm text-red-400">
                Weight (Kg's)
              </label>
              <input
                v-if="edit"
                id="weight"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.weight"
              />
              <p v-else>{{ item.weight }}</p>
            </div>
            <img
              v-if="edit"
              @click="deleteExercise(item.id)"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/bin.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addExercise"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-red-400 duration-200 border-solid border-2 border-transparent
            hover:border-red-400 hover:bg-white hover:text-red-400"
          >
            Add Exercise
          </button>
        </div>

        <!-- Cardio -->
        <div v-else-if="data.exerciseType === 'cardio'" class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cardioType" class="mb-1 text-sm text-red-400">
                Type
              </label>
              <select
                id="cardioType"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.cardioType"
              >
                <option value="#">Select Type</option>
                <option value="run">Runs</option>
                <option value="walk">Walk</option>
              </select>
              <p v-else>{{ item.cardioType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-red-400">
                Distance
              </label>
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-red-400">
                Duration
              </label>
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pace" class="mb-1 text-sm text-red-400">
                Pace
              </label>
              <input
                v-if="edit"
                id="pace"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.pace"
              />
              <p v-else>{{ item.pace }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/bin.png"
              alt=""
            />
          </div>

          <button
            @click="addExercise"
            v-if="edit"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-red-400 duration-200 border-solid border-2 border-transparent
            hover:border-red-400 hover:bg-white hover:text-red-400"
          >
            Add Exercise
          </button>
        </div>

        <!-- Sports -->
        <div v-else-if="data.exerciseType === 'sports'" class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="sportsType" class="mb-1 text-sm text-red-400">
                Type
              </label>
              <select
                id="sportsType"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
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
              <p v-else>{{ item.sportsType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="time" class="mb-1 text-sm text-red-400">
                Start Time
              </label>
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.time"
              />
              <p v-else>{{ item.time }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-red-400">
                Duration
              </label>
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="goal" class="mb-1 text-sm text-red-400">
                Goal
              </label>
              <input
                v-if="edit"
                id="pace"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.goal"
              />
              <p v-else>{{ item.goal }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/bin.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addExercise"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-red-400 duration-200 border-solid border-2 border-transparent
            hover:border-red-400 hover:bg-white hover:text-red-400"
          >
            Add Exercise
          </button>
        </div>

          <!-- Cycling -->
        <div v-else class="flex flex-col gap-y-4 w-full">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
            v-for="(item, index) in data.exercises"
            :key="index"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cyclingType" class="mb-1 text-sm text-red-400">
                Type
              </label>
              <select
                id="cyclingType"
                v-if="edit"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.cyclingType"
              >
                <option value="#">Select Type</option>
                <option value="Cycling(100W Light)">Cycling (100W Light)</option>
                <option value="Cycling(150W Moderate)">Cycling (150W Moderate)</option>
                <option value="Cycling(200W Vigorious)">Cycling (200W Vigorious)</option>
              </select>
              <p v-else>{{ item.cyclingType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-red-400">
                Distance
              </label>
              <input
                v-if="edit"
                id="distance"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.distance"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-red-400">
                Duration
              </label>
              <input
                v-if="edit"
                id="duration"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.duration"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pace" class="mb-1 text-sm text-red-400">
                Pace
              </label>
              <input
                v-if="edit"
                id="pace"
                class="p-2 w-full text-gray-500 focus:outline-none"
                type="text"
                v-model="item.pace"
              />
              <p v-else>{{ item.pace }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              class="absolute h-4 w-auto -left-5 cursor-pointer"
              src="@/assets/images/bin.png"
              alt=""
            />
          </div>
          <button
            v-if="edit"
            @click="addExercise"
            type="button"
            class="py-2 px-6 rounded-sm self-start text-sm text-white
            bg-red-400 duration-200 border-solid border-2 border-transparent
            hover:border-red-400 hover:bg-white hover:text-red-400"
          >
            Add Exercise
          </button>
        </div>
      </div>

      <!-- Update -->
      <button
        v-if="edit"
        @click="update"
        type="button"
        class="mt-10 py-2 px-6 rounded-sm self-start text-sm text-white
            bg-red-400 border-solid border-2 border-transparent
            hover:border-red-400 hover:bg-white hover:text-red-400"
      >
        Update Workout
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { supabase } from "../supabase/supabase";
import { useRoute, useRouter } from "vue-router";
import store from "../store/index";
import { uid } from "uid";
export default {
  name: "view-workout",
  setup() {
    // Create data / vars
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const router = useRouter();
    const user = computed(() => store.state.user);

    // Get current Id of route
    const currentId = route.params.workoutId;

    // Get workout data
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*")
          .eq("id", currentId);
        if (error) throw error;
        data.value = workouts[0];
        dataLoaded.value = true;
        console.log(data.value);
      } catch (error) {
        errorMsg.value = error.message;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    getData();

    // Delete workout
    const deleteWorkout = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .delete()
          .eq("id", currentId);
        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    // Edit mode
    const edit = ref(null);

    const editMode = () => {
      edit.value = !edit.value;
    };

    // Add exercise
    const addExercise = () => {
      if (data.value.exerciseType === "strength") {
        data.value.exercises.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      } else if (data.value.exerciseType === "cardio") {
      data.value.exercises.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      });
      return;
    } else if (data.value.exerciseType === "sports") {
      data.value.exercises.push({
        id: uid(),
        sportsType: "",
        time: "",
        duration: "",
        pace: "",
      });
      return;
    } else
      data.value.exercises.push({
        id: uid(),
        cyclingType: "",
        distance: "",
        duration: "",
        pace: "",
      });
    };

    // Delete exercise
    const deleteExercise = (id) => {
      if (data.value.exercises.length > 1) {
        data.value.exercises = data.value.exercises.filter(
          (workout) => workout.id !== id
        );
        return;
      }
      errorMsg.value = "Cannot remove, need to at least have one exercise";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // Update Workout
    const update = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .update({
            exerciseName: data.value.exerciseName,
            exercises: data.value.exercises,
          })
          .eq("id", currentId);
        if (error) throw error;
        edit.value = false;
        statusMsg.value = "Workout Updated!";
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value`${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return {
      statusMsg,
      data,
      dataLoaded,
      errorMsg,
      edit,
      editMode,
      user,
      deleteWorkout,
      addExercise,
      deleteExercise,
      update,
    };
  },
};
</script>
