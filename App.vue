<template>
  <div id="app">
    <div class="table-container">
      <h2>Študenti</h2>
      <table>
        <thead>
          <tr>
            <th>Meno</th>
            <th>Známka</th>
            <th>Akcie</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(student, index) in students" :key="index">
            <td>{{ student.meno }}</td>
            <td>{{ student.znamka }}</td>
            <td>
              <button @click="removeStudent(index)">Odstrániť</button>
            </td>
          </tr>
        </tbody>
      </table>

      <p v-if="students.length === 0">Nenašli sa žiadni študenti.</p>

      <div class="form-container">
        <h2>Pridať nového študenta</h2>

        <form @submit.prevent="addStudent">
          <label for="meno">Meno:</label>
          <input v-model="newStudent.meno" type="text" id="meno" required pattern="[A-Za-z ]{3,}">
          
          <label for="znamka">Známka:</label>
          <input v-model.number="newStudent.znamka" type="number" id="znamka" required min="0" max="100">

          <button type="submit">Pridať študenta</button>
        </form>

        <h3>Priemerná známka: {{ averageGrade }}</h3>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      students: [
        { meno: 'Adam', znamka: 90 },
        { meno: 'Miro', znamka: 85 },
        { meno: 'Jano', znamka: 88 },
      ],
      newStudent: { meno: '', znamka: 0 },
    };
  },
  computed: {
    averageGrade() {
      if (this.students.length === 0) return 0;

      const totalGrade = this.students.reduce((sum, student) => sum + student.znamka, 0);
      return (totalGrade / this.students.length).toFixed(2);
    },
  },
  methods: {
    addStudent() {
      if (this.isValidStudent()) {
        this.students.push({ meno: this.newStudent.meno, znamka: this.newStudent.znamka });
        this.newStudent = { meno: '', znamka: 0 };
      } else {
        alert('Neplatné údaje. Skontrolujte meno (min. 3 znaky) a známku (0-100).');
      }
    },
    removeStudent(index) {
      this.students.splice(index, 1);
    },
    isValidStudent() {
      const isValidName = /^[A-Za-z ]{3,}$/.test(this.newStudent.meno);
      const isValidGrade = Number.isInteger(this.newStudent.znamka) && this.newStudent.znamka >= 0 && this.newStudent.znamka <= 100;

      return isValidName && isValidGrade;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: 'Georgia', serif;
  max-width: 900px;
  margin: auto;
  padding: 30px;
  text-align: center; /* Add this line to center everything */
}

h2, h3 {
  color: #800000;
}

.table-container {
  width: 100%; /* Change this from 80% to 100% to make the table wider */
  margin: auto;
}

table {
  width: 100%;
}

th, td {
  border: 1px solid #999999;
  padding: 10px;
}

th {
  background-color: #cccccc;
}

.form-container {
  width: 100%;
}

form {
  width: inherit;
}

button {
  background-color: #800000;
}
</style>
