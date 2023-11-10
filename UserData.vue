<template>
    <div>
      <h2>User Data</h2>
      <table>
        <thead>
          <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Date of Birth</th>
            <th>Email</th>
            <th>Residence Address</th>
            <!-- <th>Qualification</th> -->
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(user,index) in userData" :key="index">
            <td>{{ user.firstName }}</td>
            <td>{{ user.lastName }}</td>
            <td>{{ user.dateOfBirth }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.residenceAddress }}</td>
            <!-- <td>{{ user.qualification }}</td> -->
            <td>
              <button @click="deleteUser(user._id,index)">Delete</button>
              <button @click="editUser(user, index)">Update</button>
            </td>
          </tr>
        </tbody>
      </table>

    <div v-if="showEditForm">
      <h3>Edit User</h3>
      <input v-model="editedUser.firstName"  />
      <input v-model="editedUser.lastName"  />
      <input v-model="editedUser.dateOfBirth"  />
      <input v-model="editedUser.email"  />
      <input v-model="editedUser.residenceAddress" />
      <button @click="updateUser()">Update</button>
    </div>

    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        userData: [],
        showEditForm : false,
        editedUser : {},
        editedIndex : null
      };
    },
    // mounted() {
    //   // Fetch user data on component mount
    //   this.fetchUserData();
    //   // this.deleteUser();
    // },
    methods: {
      fetchUserData() 
    {
        fetch('http://localhost/mongodbphp/savedataog.php', {
         method: 'GET',
    })
    .then(response => {
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      return response.json();
    })
    .then(data => {
      this.userData = data; // Assuming the response data is an array of users
    })
    .catch(error => {
      console.error('Error fetching user data:', error);
    });
    },

    deleteUser(userId, index) {
      fetch(`http://localhost/mongodbphp/savedataog.php/${userId}`, {
        method: 'DELETE',
      })
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          this.userData.splice(index, 1); // Remove the user from the array on successful deletion
        })
        .catch(error => {
          console.error('Error deleting user:', error);
        });
    },
  
    editUser(user, index) {
      this.editedUser = { ...user };
      this.editedIndex = index;
      this.showEditForm = true;
    },

    updateUser() {
    const updatedUser = this.editedUser;
    fetch(`http://localhost/mongodbphp/savedataog.php/${updatedUser._id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(updatedUser),
    })
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        this.showEditForm = false; // Hide the edit form after updating
        return ; // Get the updated user data
      })
      .then(data => {
        // Find the index of the updated user in the userData array
        alert('worked');
        const index = this.userData.findIndex(user => user._id === updatedUser._id);
        this.fetchUserData();
        if (index !== -1) {
          // Replace the old user data with the updated data
          this.userData.splice(index, 1, data);
        }
      })
      .catch(error => {
        console.error('Error updating user:', error);
      });
  },
},

  mounted() {
    // Fetch user data from the backend and populate this.userData array
    // ...
    this.fetchUserData();
    },
  };
  </script>

<style scoped>

table , td , th , thead , tbody{
    border: 2px black solid;
    border-collapse: collapse;
    padding: 20px;
  }

  /* tr{
    border: 5px black solid;
    padding: 15px;
  } */

</style>