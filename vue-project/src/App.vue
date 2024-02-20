import './assets/css/main.css'

<template>
  <div>
    <form @submit.prevent="addExercise">
    <select v-model="newExercise.type">
        <option disabled value="">Select an exercise</option>
        <option>Bench Press</option>
        <option>Lunges</option>
        <option>Running</option>
        <option>Squats</option>
        <option>Wall Sit</option>
      </select>

      <select v-model="newExercise.type">
        <option disabled value="">Please select one</option>
        <option>Time</option>
        <option>Sets</option>
      </select>
      <input type="date" v-model="newExercise.date" required>
      <input type="number" v-model="newExercise.time" v-if="newExercise.type === 'Time'" required>
      <input type="number" v-model="newExercise.sets" v-if="newExercise.type === 'Sets'" required>
      <input type="number" v-model="newExercise.reps" v-if="newExercise.type === 'Sets'" required>
      <input type="number" v-model="newExercise.weight" v-if="newExercise.type === 'Sets'" required>
      <button type="submit">Add Exercise</button>
    </form>
    <ul>
      <li v-for="(exercise, index) in exercises" :key="index">
        <span>{{ exercise.date }} - {{ exercise.type }} - {{ exercise.time || exercise.sets + ' sets ' + exercise.reps + ' reps ' + exercise.weight + ' kg' }}</span>
        <button @click="editExercise(index)">Edit</button>
        <button @click="deleteExercise(index)">Delete</button>
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
      this.exercises.push(this.newExercise);
      this.newExercise = { type: '', date: '', time: '', sets: '', reps: '', weight: '' };
    },
    deleteExercise(index) {
      this.exercises.splice(index, 1);
    },
    editExercise(index) {
      this.newExercise = this.exercises[index];
      this.deleteExercise(index);
    }
  }
};
</script>