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
              <button @click="deleteUser(user._id,index)" @submit="deleteUser(user._id,index)">Delete</button>
              <button @click="editUser(user, index)">Update</button>
            </td>
          </tr>
        </tbody>
      </table>

<div v-if="showEditForm">
   <h3>Edit User</h3>
   <input v-model="editedUser.firstName" @input="updateEditedUserProperty('firstName', $event)" />
   <input v-model="editedUser.lastName" @input="updateEditedUserProperty('lastName', $event)" />
   <input v-model="editedUser.dateOfBirth" @input="updateEditedUserProperty('dateOfBirth', $event)" />
   <input v-model="editedUser.email" @input="updateEditedUserProperty('email', $event)" />
   <input v-model="editedUser.residenceAddress" @input="updateEditedUserProperty('residenceAddress', $event)" />
   <button @click="updateUser">Update</button>
</div>

    </div>
  </template>
  
  <script>
//  import axios from "axios";

  export default {
     data() {
      return {
        userData: {},
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
        fetch('http://localhost/mongodbphp/savedata.php', {
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
    let uid = userId.$oid;

    // Assuming userData is an array that represents your table data
    // and you want to remove the entry at the specified index
    this.userData.splice(index, 1);

    // Update the UI immediately, and then send the DELETE request
    fetch(`http://localhost/mongodbphp/savedata.php?userId=${uid}`, {
        method: 'DELETE',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ userId: uid }),
    })
    .then(response => response.json())
    .then(data => {
        // Check the response from the server
        console.log(data);

        if (data.message === 'User deleted successfully') {
            // Optional: You can handle success if needed
            console.log('User deleted successfully');
        } else {
            // Optional: Handle the case where the server reports an error
            console.error('Error deleting user:', data.message);
        }
    })
    .catch(error => {
        // Optional: Handle fetch error
        console.error('Error deleting user:', error);
    });
},


    editUser(user, index) {
      this.editedUser = { ...user };
      alert(this.editedUser , this.editedIndex)
      this.editedIndex = index;
      this.showEditForm = true;
    },

    updateUser() {
      const updatedUser = { ...this.editedUser }; // create a copy to avoid reactivity issues
      fetch(`http://localhost/mongodbphp/savedata.php/${updatedUser._id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updatedUser),
      })
        .then((response) => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then((data) => {
          // const index = this.userData.findIndex(
          //   (user) => user._id === updatedUser._id
          // );
        
          this.userData.splice(this.editedIndex, 1, data);

          this.showEditForm = false;
          this.editedUser = {};
          this.editedIndex = null;
          // this.fetchUserData();
          alert('Success')
        })
        .catch((error) => {
          console.error('Error updating user:', error);
        });
    },

    updateEditedUserProperty(property, event) {
      const value = event.target.value;
   this.editedUser[property] = value;
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