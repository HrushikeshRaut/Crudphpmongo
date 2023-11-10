<template>

  <div class="hello">
    <h1>Admission form</h1>
    <form @submit.prevent="submitForm">
    <table align="center">
    
      <tr>
        <td><label for="">First name :&nbsp;&nbsp;&nbsp;</label></td>
        <td><input v-model="formData.firstName" type="text" ></td>
      </tr>
    <!-- <br><br> -->
    <tr>
      <td><label for="">Last name :&nbsp;&nbsp;&nbsp;</label></td>
      <td><input v-model="formData.lastName" type="text" name="" id=""></td>
    </tr>
    <!-- <br><br> -->
    <tr>
       <td><label for="" placeholder="DD-MM-YYYY"> Date of birth :&nbsp;&nbsp; </label></td>
       <td><input v-model="formData.dateOfBirth" type="date" name="" id="date" ></td>
    </tr>
    <!-- <br><br> -->
    <tr>
       <td><label for="">Email address : </label></td>
       <td><input v-model="formData.email" type="email" name="" id=""></td>
    </tr>
    <!-- <br><br> -->
    <tr>
      <td><label for="">Resident adress : </label></td>
      <td><input v-model="formData.residenceAddress" type="text"></td>
    </tr>
    <!-- <br><br> -->
    <!-- <tr>
      <td><label for=""> Qualification : </label></td>
      <td>
      <select name="" id="" v-model="formData.qualification">
      <option value="">High School</option>
      <option value="">Inter</option>
      <option value="">Graduate</option>
      <option value="">Post Graduate</option>
      </select>
      </td>
    </tr> -->
    <!-- <br><br> -->
    <tr>
    <td colspan="2"><button type="submit">Click to add</button></td>
    </tr>
  </table>
  </form>

  <!-- <div v-if="userData"> -->
      <!-- <h2>User Data</h2> -->
      <!-- Render your table with userData -->
      <!-- <table> -->
        <!-- Table header goes here -->
        <!-- <thead> -->
          <!-- Table header rows go here -->
        <!-- </thead>
        <tbody> -->
          <!-- Iterate over userData and display rows -->
          <!-- <tr v-for="(user, index) in userData" :key="index"> -->
            <!-- Display user data in table rows -->
          <!-- </tr>
        </tbody>
      </table>
    </div> -->

</div>

</template>

<script>
import axios from "axios";

export default {
  data() {
  return {
    formData:{
      firstName : '',
      lastName : '',
      dateOfBirth : '',
      email : '',
      residenceAddress : '',
      //qualification : ''
    },
    // userData:null,
  };
  },
  methods: {
    submitForm() {
      // Client-side validation of form data
      this.errors = {}
      console.log(this.formData)
      if (!this.formData.firstName || 
      !this.formData.lastName || 
      !this.formData.dateOfBirth|| 
      !this.formData.email|| 
      !this.formData.residenceAddress||
      this.formData.qualification 
      // !this.formData.qualification
      ) 
      {
        // Display error messages or prevent form submission if data is not valid.
        console.log( JSON.stringify(this.errors));
        alert("Feel all the details...")
        return;
      }

      // Send the form data to the server (PHP backend) for processing
      axios.post('http://localhost/mongodbphp/savedata.php', JSON.stringify(this.formData) , { withCredentials: true })
        .then(response => {
          // Handle the response from the server (e.g., success message or error message).
          alert("submited successfully")
          console.log(response.data);
          //this.fetchUserData(this.userData);
          // Optionally, reset the form or perform other client-side actions.
        })
        .catch(errors => {
          // Handle errors that may occur during the HTTP request.
          console.log(errors);
        });
        },
        //fetchUserData() {
  // fetch('http://localhost:8080')
  //   .then(response => {
  //     // Check if the response is in JSON format
  //     if (response.headers.get('content-type')?.includes('application/json')) {
  //       return response.json();
  //     } else {
  //       // If not JSON, handle the response accordingly (e.g., display an error message)
  //       throw new Error('Non-JSON response received');
  //     }
  //   })
//     .then(parsedData => {
//       // Emit an event to notify the parent component
//       this.$emit('updateUserData', parsedData);
//     })
//     .catch(error => {
//       console.error('Error fetching user data:', error);
//     });
// },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
body{
  margin: 0%;
  background-color: aquamarine;
}

input {
    /* width: calc(60% - 40px);
    border: 0 black 5px; */
    /* padding: 20px; */
    font-size: 1.3em;
    color : black;
    /* background-color: #07bebe; */
  }

  label{
    font-size: larger;
    font-family: Georgia, 'Times New Roman', Times, serif;
  }

  table , td {
    /* border: 2px black solid; */
    border-collapse: collapse;
    /* padding: 15px; */
  }

  td{
    /* border: 5px black solid; */
    padding: 15px;
  }

button{
  padding: 20px;
  color:red
}

button:hover{
  background-color: rgb(17, 49, 69);
  cursor: pointer;
}

#date{
  width: 100%;
}
</style>
