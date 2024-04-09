import './assets/base.css'

<template>
  <div class="inline-block relative w-full">
    <form @submit.prevent="addExercise">
    <select v-model="newExercise.type" class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
        <option disabled value="">Select an exercise</option>
        <option>Bench Press</option>
        <option>Lunges</option>
        <option>Running</option>
        <option>Squats</option>
        <option>Wall Sit</option>
      </select>

      <select v-model="newExercise.length" class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
        <option disabled value="">Select a Type</option>
        <option>Time</option>
        <option>Sets</option>
      </select>

      <input type="date" v-model="newExercise.date" required class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">

    <label v-if="newExercise.length === 'Time'" for="time">Time:</label>
      <input id="time" type="number" v-model="newExercise.time" v-if="newExercise.length === 'Time'" required class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
    <label v-if="newExercise.length === 'Sets'" for="sets">Sets:</label>
      <input type="number" v-model="newExercise.sets" v-if="newExercise.length === 'Sets'" required class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
      <label v-if="newExercise.length === 'Sets'" for="reps">Reps:</label>
      <input type="number" v-model="newExercise.reps" v-if="newExercise.length === 'Sets'" required class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
      <label v-if="newExercise.length === 'Sets'" for="weight">Weight:</label>
      <input type="number" v-model="newExercise.weight" v-if="newExercise.length === 'Sets'" required class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
      <button class="shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" type="submit">Add Exercise</button>
    </form>
    <ul>
      <li v-for="(exercise, index) in exercises" :key="index">
       <template v-if="!exercise.isEditing">
        <span>{{ exercise.date }} - {{ exercise.type }} - {{ exercise.time + ' minutes ' || exercise.sets + ' sets ' + exercise.reps + ' reps ' + exercise.weight + ' kg' }}</span>
        <button class="ml-2 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800 edit-button" @click="editExercise(index)">Edit</button>
        <button class="ml-2 focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900" @click="deleteExercise(index)">Delete</button>
      </template>
<template v-else>
  <form @submit.prevent="updateExercise(index)">
  <select v-model="exercise.type" class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline" required>
              <option disabled value="">Select an exercise</option>
              <option>Bench Press</option>
              <option>Lunges</option>
              <option>Running</option>
              <option>Squats</option>
              <option>Wall Sit</option>
            </select>

             <select v-model="exercise.length" class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline" required>
              <option disabled value="">Select a Type</option>
              <option>Time</option>
              <option>Sets</option>
            </select>

             <input type="date" v-model="exercise.date" required class="m-2 block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">

    <label v-if="exercise.length === 'Time'" for="time-edit">Time:</label>
    <input id="time-edit" type="number" v-model="exercise.time" v-if="exercise.length === 'Time'" required>

    <label v-if="exercise.length === 'Sets'" for="sets-edit">Sets:</label>
    <input id="sets-edit" type="number" v-model="exercise.sets" v-if="exercise.length === 'Sets'" required>

    <label v-if="exercise.length === 'Sets'" for="reps-edit">Reps:</label>
    <input id="reps-edit" type="number" v-model="exercise.reps" v-if="exercise.length === 'Sets'" required>

    <label v-if="exercise.length === 'Sets'" for="weight-edit">Weight:</label>
    <input id="weight-edit" type="number" v-model="exercise.weight" v-if="exercise.length === 'Sets'" required>

            <button class="shadow bg-green-500 hover:bg-green-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" type="submit">Update Exercise</button>
  </form>
</template>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      exercises: [],
      newExercise: {
        type: '',
        date: '',
        time: '',
        sets: '',
        reps: '',
        weight: ''
      }
    };
  },
 methods: {
    addExercise() {
      this.exercises.push({ ...this.newExercise, isEditing: false });
      this.newExercise = { type: '', date: '', time: '', sets: '', reps: '', weight: '', length: '' };
    },
    editExercise(index) {
      this.exercises[index].isEditing = true;
    },
    updateExercise(index) {
      this.exercises[index].isEditing = false;
    },
    deleteExercise(index) {
      this.exercises.splice(index, 1);
    }
  }
};
</script>

