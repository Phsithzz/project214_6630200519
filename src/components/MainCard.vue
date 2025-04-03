<template>
  <header>
    <a href="#" class="logo">Phoosith Pheanchob</a>
    <nav>
        <a href="#home">Home</a>
        <a href="#education">Education</a>
        <a href="#academic_performance">Academic Performance</a>
    </nav>
  </header>

  
  <div id="home" class="main-container">
    <div class="container-one">
      <div class="profile">
        <img src="../assets/img4.jpg" alt="Profile Picture">
      </div>
    </div>

    <div class="container-two">
      <div class="about-me">
        <h1 class="heads">My Profile</h1>
        <h3 class="n">{{ userData.name }}</h3>
        <h3 class="numnisit">{{ userData.studentId }}</h3>
        <h3 class="n">{{ userData.major }}</h3>
        <button @click="toggleEditProfile" class="edit-button">Edit Profile</button> 
      </div>
    </div>
  </div>

  
  <div id="education" class="education">
    <h2>My Education</h2>
    <div class="education-content">
      <div class="education-item">
        <img src="../assets/school1.jpg" height="200" width="200">
        <h3 class="n2">{{ educationData.primarySchool.name }}</h3>
        <p class="n3">{{ educationData.primarySchool.year }}</p>
        <button @click="toggleEditEducation('primarySchool')" class="edit-button">Edit Education</button>
      </div>
      <div class="education-item">
        <img src="../assets/school2.png" height="200" width="200">
        <h3 class="n2">{{ educationData.secondarySchool.name }}</h3>
        <p class="n3">{{ educationData.secondarySchool.year }}</p>
        <button @click="toggleEditEducation('secondarySchool')" class="edit-button">Edit Education</button>
      </div>
      <div class="education-item">
        <img src="../assets/school3.png" height="200" width="200">
        <h3 class="n2">{{ educationData.university.name }}</h3>
        <p class="n3">{{ educationData.university.year }}</p>
        <button @click="toggleEditEducation('university')" class="edit-button">Edit Education</button>
      </div>
    </div>
  </div>

 
<div id="academic_performance" class="academic-performance">
  <h2>Academic Performance</h2>
  
  <img src="../assets/img5.jpg" alt="" class="pic2">
  <h3 class="n6">ผลการเรียนนิสิตมหาวิทยาลัยเกษตรศาสตร์ วิทยาเขตศรีราชา</h3>
  <table class="performance-table">
    <thead>
      <tr class="t">
        <th>รหัสวิชา</th>
        <th>ชื่อวิชา</th>
        <th>หน่วยกิต</th>
        <th>เกรด</th>
        <th>EDIT/DELETE</th>
      </tr>
    </thead>
    <tbody>
      <tr class="t1" v-for="(item, index) in academicPerformanceData" :key="index">
        <td>{{ item.courseCode }}</td>
        <td>{{ item.courseName }}</td>
        <td>{{ item.credits }}</td>
        <td>{{ item.grade }}</td>
        <td>
          <button @click="toggleEditAcademicPerformance(item)" class="performance-button">Edit</button>
          <button @click="deleteAcademicPerformance(item.id)" class="performance-button delete">Delete</button>
        </td>
      </tr>
    </tbody>
  </table>
  <button @click="toggleAddAcademicPerformance" class="performance-button">Add New</button>
</div>

  
  <div v-if="isEditingProfile" class="modal-overlay" @click.self="cancelEditProfile">
    <div class="modal-content">
      <h2 class="e1">Edit Profile</h2>
      <div class="edit-form">
        <label for="name">Name:</label>
        <input id="name" v-model="editedData.name" type="text" required />
        <label for="studentId">Student ID:</label>
        <input id="studentId" v-model="editedData.studentId" type="text" required />
        <label for="major">Major:</label>
        <input id="major" v-model="editedData.major" type="text" required />
        <div class="modal-buttons">
          <button @click="saveProfileChanges" class="save-button">Save</button>
          <button @click="cancelEditProfile" class="cancel-button">Cancel</button>
        </div>
      </div>
    </div>
  </div>

 
  <div v-if="isEditingEducation" class="modal-overlay" @click.self="cancelEditEducation">
    <div class="modal-content">
      <h2 class="e1">Edit Education</h2>
      <div class="edit-form">
        <label for="schoolName">School Name:</label>
        <input v-model="editedEducation.name" type="text" required />
        <label for="year">Year:</label>
        <input v-model="editedEducation.year" type="text" required />
        <div class="modal-buttons">
          <button @click="saveEducationChanges" class="save-button">Save</button>
          <button @click="cancelEditEducation" class="cancel-button">Cancel</button>
        </div>
      </div>
    </div>
  </div>
  <div v-if="isEditingAcademicPerformance" class="modal-overlay" @click.self="cancelEditAcademicPerformance">
    <div class="modal-content">
      <h2 class="e1">Edit Academic Performance</h2>
      <div class="edit-form">
        <label for="courseCode">Course Code:</label>
        <input id="courseCode" v-model="editedAcademicPerformance.courseCode" type="text" required />
        <label for="courseName">Course Name:</label>
        <input id="courseName" v-model="editedAcademicPerformance.courseName" type="text" required />
        <label for="credits">Credits:</label>
        <input id="credits" v-model="editedAcademicPerformance.credits" type="number" required />
        <label for="grade">Grade:</label>
        <input id="grade" v-model="editedAcademicPerformance.grade" type="text" required />
        <div class="modal-buttons">
          <button @click="saveAcademicPerformanceChanges" class="save-button">Save</button>
          <button @click="cancelEditAcademicPerformance" class="cancel-button">Cancel</button>
        </div>
      </div>
    </div>
  </div>


  <div v-if="isAddingAcademicPerformance" class="modal-overlay" @click.self="cancelAddAcademicPerformance">
    <div class="modal-content">
      <h2 class="e1">Add New Academic Performance</h2>
      <div class="edit-form">
        <label for="newCourseCode">Course Code:</label>
        <input id="newCourseCode" v-model="newAcademicPerformance.courseCode" type="text" required />
        <label for="newCourseName">Course Name:</label>
        <input id="newCourseName" v-model="newAcademicPerformance.courseName" type="text" required />
        <label for="newCredits">Credits:</label>
        <input id="newCredits" v-model="newAcademicPerformance.credits" type="number" required />
        <label for="newGrade">Grade:</label>
        <input id="newGrade" v-model="newAcademicPerformance.grade" type="text" required />
        <div class="modal-buttons">
          <button @click="saveNewAcademicPerformance" class="save-button">Save</button>
          <button @click="cancelAddAcademicPerformance" class="cancel-button">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      isEditingProfile: false,
      isEditingEducation: false,
      educationType: '', 
      userData: {},
      educationData: {
        primarySchool: { name: 'โรงเรียนนภสร', year: '2553 - 2559' },
        secondarySchool: { name: 'โรงเรียนเฉลิมพระเกียรติ60พรรษา', year: '2560 - 2565' },
        university: { name: 'มหาวิทยาลัยเกษตรศาสตร์ วิทยาเขตศรีราชา', year: 'ศึกษาอยู่ ณ ปัจจุบัน' }
      },
      academicPerformanceData: "Here",
      editedData: {
        name: '',
        studentId: '',
        major: ''
      },
      editedEducation: {
        name: '',
        year: ''
      },
        academicPerformanceData: [],
      isEditingAcademicPerformance: false,
      isAddingAcademicPerformance: false,
      editedAcademicPerformance: {},
      newAcademicPerformance: {
        courseCode: '',
        courseName: '',
        credits: '',
        grade: ''
      }
    };
  },
  created() {
    this.fetchUserData();
    this.fetchAcademicPerformanceData();
  },
  methods: {
    async fetchUserData() {
      try {
        const response = await axios.get('/data/db.json');
        this.userData = response.data;
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    toggleEditProfile() {
      this.isEditingProfile = true;
      this.editedData = { ...this.userData };
    },
    toggleEditEducation(educationType) {
      this.isEditingEducation = true;
      this.educationType = educationType;
      this.editedEducation = { ...this.educationData[educationType] };
    },
    async saveProfileChanges() {
      if (!this.editedData.name || !this.editedData.studentId || !this.editedData.major) {
        alert('Please fill out all fields.');
        return;
      }
      try {
        await axios.put('/data/db.json', this.editedData);
        this.userData = { ...this.editedData };
        this.isEditingProfile = false;
      } catch (error) {
        console.error('Error saving data:', error);
      }
    },
    cancelEditProfile() {
      this.isEditingProfile = false;
    },
    saveEducationChanges() {
      if (!this.editedEducation.name || !this.editedEducation.year) {
        alert('Please fill out all fields.');
        return;
      }
      this.educationData[this.educationType] = { ...this.editedEducation };
      this.isEditingEducation = false;
    },
    cancelEditEducation() {
      this.isEditingEducation = false;
    },
    async fetchAcademicPerformanceData() {
  try {
    const response = await axios.get('http://localhost:3000/academicPerformance');
    this.academicPerformanceData = response.data;
  } catch (error) {
    console.error('Error fetching academic performance data:', error);
  }
},

    async saveAcademicPerformanceChanges() {
  if (!this.editedAcademicPerformance.courseCode || !this.editedAcademicPerformance.courseName || !this.editedAcademicPerformance.credits || !this.editedAcademicPerformance.grade) {
    alert('Please fill out all fields.');
    return;
  }

  try {
    const response = await axios.put(`http://localhost:3000/academicPerformance/${this.editedAcademicPerformance.id}`, this.editedAcademicPerformance);
    if (response.status === 200) {
      this.fetchAcademicPerformanceData();  
      this.isEditingAcademicPerformance = false;  
    } else {
      console.error('Error updating academic performance:', response);
    }
  } catch (error) {
    console.error('Error saving academic performance changes:', error);
  }
},

    cancelEditAcademicPerformance() {
      this.isEditingAcademicPerformance = false;
    },
    async deleteAcademicPerformance(id) {
  if (confirm('Are you sure you want to delete this academic performance?')) {
    try {
      const response = await axios.delete(`http://localhost:3000/academicPerformance/${id}`);
      if (response.status === 200) {
        this.fetchAcademicPerformanceData();  
      } else {
        console.error('Error deleting academic performance:', response);
      }
    } catch (error) {
      console.error('Error deleting academic performance:', error);
    }
  }
},

    toggleEditAcademicPerformance(item) {
      this.isEditingAcademicPerformance = true;
      this.editedAcademicPerformance = { ...item };
    },
    toggleAddAcademicPerformance() {
      this.isAddingAcademicPerformance = true;
    },
    async saveNewAcademicPerformance() {
      try {
        await axios.post('http://localhost:3000/academicPerformance', this.newAcademicPerformance);
        this.fetchAcademicPerformanceData();
        this.isAddingAcademicPerformance = false;
      } catch (error) {
        console.error('Error adding new academic performance:', error);
      }
    },
    cancelAddAcademicPerformance() {
      this.isAddingAcademicPerformance = false;
    }
  }
};
</script>

<style scoped>

header {
  margin-top: 20px;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 1rem 9%;
  background-color: transparent;
  display: flex;
  justify-content: flex-start; 
  align-items: center;
  z-index: 100;
}

.logo {
  font-size: 3rem;
  color: #6495ED;
  font-weight: 800;
  cursor: pointer;
  transition: 0.5s ease;
  font-family: Permanent Marker;
}

.logo:hover {
  transform: scale(1.1);
}

nav a {
  font-size: 1.5rem; 
  color: black;
  margin-left: 2rem;
  font-weight: 500;
  transition: 0.3s ease;
  border-bottom: 3px solid transparent;
  font-family: Rowdies;
}


nav a:hover {
  border-bottom: 3px solid #6495ED;
}


.profile img {
  border-radius: 50%;
  object-fit: cover;
  width: 500px;
  height: 500px;
}
.profile img:hover{
  transition: 1s ease;
  transform: scale(1.1);
}
.pic2{
   border-radius: 50%;
  object-fit: cover;
  width: 500px;
  height: 500px;
}
.pic2:hover{
  transition: 1s ease;
  transform: scale(1.1);
}

.main-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 20px;
}

.container-one, .container-two {
  width: 100%;
  max-width: 600px;
}
.n{
  font-size: 3rem;
  font-weight: 700;
  line-height: 1.3;
  font-family: Srisakdi;
}
.numnisit{
   font-size: 3rem;
  font-weight: 700;
  line-height: 1.3;
  font-family: Roboto Mono;
}
.n6{
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1.3;
  font-family: Roboto Mono;
}

.n2{
   font-size: 2rem;
    color: #6495ED;
    margin-bottom: 1rem;
    font-family: Srisakdi;
}
.n3{
  font-size: 1rem;
    line-height: 1.5;
    color: black;
    font-family: Sriracha;
}
.profile {
  text-align: center;
}

.edit-button {
  background-color: #3949ab;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 15px;
  font-size: 1rem;
  font-family: Roboto Mono;
}
.edit-button:hover{
    transform: scale(1.05);
    transition: 0.3s ease;
    box-shadow: 0 0 25px #CC0033;
}


.education {
  padding: 50px;
  margin-top: 100px;
  background-color: #f8f9fa;
  text-align: center;
}

.education-content {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  gap: 20px;
}

.education-item {
    background-color: #f5f5f5;
    padding: 2rem;
    border-radius: 1rem;
    box-shadow: 0 0 15px #6495ED;
    max-width: 600px;
    width: 100%;
    transition: 0.3s ease;
}
.education-item:hover {
  transform: scale(1.05); 
  box-shadow: 0 0 15px rgba(0, 0, 255, 0.2); 
  transition: transform 0.3s ease, box-shadow 0.3s ease; 
}

.education h2 {
  font-size: 5rem;
  color: #6495ED;
  margin-bottom: 3rem;
  text-transform: uppercase;
  font-weight: 700;
  font-family: Permanent Marker;
}
.education h2:hover{
  transition: 0.5s ease;
  transform: scale(1.05);
}



.academic-performance {
  padding: 50px;
  margin-top: 100px;
  background-color: #e9ecef;
  text-align: center;
}

.academic-performance h2 {
  font-size: 4rem;
  color: #6495ED;
  margin-bottom: 3rem;
  font-family: Permanent Marker;
}


.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  max-width: 400px;
  width: 100%;
  text-align: center;
}

.edit-form {
  display: flex;
  flex-direction: column;
}

.edit-form label {
  margin-bottom: 5px;
  font-weight: bold;
  font-family: Roboto Mono;
}

.edit-form input {
  margin-bottom: 15px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

.heads{
 
    font-size: 5rem;
    color: #6495ED;
    margin-bottom: 3rem;
    text-transform: uppercase;
    font-weight: 700;
    font-family: Permanent Marker;


}
.heads:hover {
  transition: 0.5s ease;
  transform: scale(1.1);
}

.modal-buttons {
  display: flex;
  justify-content: space-between;
}

.save-button, .cancel-button {
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  font-family: Roboto Mono;
}

.save-button {
  background-color: #28a745;
  color: white;
  border: none;
  font-family: Roboto Mono;
}

.cancel-button {
  background-color: #dc3545;
  color: white;
  border: none;
  font-family: Roboto Mono;
}

.save-button:hover {
  background-color: #218838;
  transform: scale(1.1);
}

.cancel-button:hover {
  background-color: #c82333;
  transform: scale(1.1);
}

a {
  text-decoration: none;
}

.t1{
  font-family: Roboto Mono;
}
.academic-performance {
  padding: 50px;
  margin-top: 100px;
  background-color: #e9ecef;
  text-align: center;
  border-radius: 1rem;
  
}

.academic-performance h2 {
  font-size: 4rem;
  color: #6495ED;
  margin-bottom: 3rem;
  font-family: Permanent Marker;
}
.academic-performance h2:hover {
  transition: 0.5s ease;
  transform: scale(1.1);
}


.t{
  font-family: Sriracha;
}
.performance-table {
  width: 100%;
  border-collapse: collapse;
  margin: 0 auto;
  background-color: #fff;
  
  border-radius: 10px;
}

.performance-table th, .performance-table td {
  padding: 12px;
  border: 1px solid #ccc;
  text-align: center;
  font-size: 1rem;
}

.performance-table th {
  background-color: #6495ED;
  color: white;
  font-size: 1.2rem;
  text-transform: uppercase;
}

.performance-table td {
  background-color: #f9f9f9;
}


.performance-table tr:hover {
  background-color: #f1f1f1;
}


.performance-button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.3s ease;
  font-family: Roboto Mono;
}
.performance-table td button {
  margin-right: 10px; 
}

.performance-table td .performance-button.delete {
  margin-left: 10px; 
}


.performance-button:hover {
  transform: scale(1.05);
  transition: 0.3s ease;
  box-shadow: 0 0 25px #CC0033;
  background-color: #0056b3;
}

.performance-button.delete {
  background-color: #dc3545;
  font-family: Roboto Mono;
}

.performance-button.delete:hover {
  background-color: #c82333;
}

.academic-performance button {
  background-color: #3949ab;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  margin-top: 20px;
  transition: background-color 0.3s ease;
}

.academic-performance button:hover {
  background-color: #3949ab;
}
.e1{
  font-family: Roboto Mono;
}
</style>
