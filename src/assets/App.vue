<template>
  <div id="app">
    <div class="section">
      <p>Profiles List</p>
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input class="input" v-model="searchQuery" @input="filterProfiles" />
      </div>
      <div class="buttons">
        <button @click="sortAsc">&#9650;</button>
        <button @click="sortDesc">&#9660;</button>
      </div>

      <ProfileCard
        v-for="(profile, index) in filteredProfiles"
        :key="profile.id"
        :profile="profile"
        @add-comment="addComment"
        class="profile"
      />
    </div>

    <div class="section">
      <p class="header">Add new profile</p>
      <div class="flex-row">
        <label class="label">Name:</label>
        <input class="input" v-model="newProfile.name" />
      </div>
      <div class="flex-row">
        <label class="label">Email:</label>
        <input class="input" v-model="newProfile.email" />
      </div>
      <div class="flex-row">
        <label class="label">Specialisation:</label>
        <input class="input" v-model="newProfile.description" />
      </div>
      <button @click="addProfile">Add</button>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard.vue";

export default {
  name: "App",

  components: {
    ProfileCard,
  },

  data() {
    return {
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34,
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28,
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
        },
      ],
      comments: {},
      searchQuery: "",
      newProfile: {
        name: "",
        email: "",
        description: "",
      },
      filteredProfiles: [],
    };
  },

  created() {
    this.filteredProfiles = this.profiles;
  },

  methods: {
    sortAsc() {
      this.filteredProfiles.sort((a, b) => a.likes - b.likes);
    },

    sortDesc() {
      this.filteredProfiles.sort((a, b) => b.likes - a.likes);
    },

    addComment({ id, comment }) {
      if (!this.comments[id]) {
        this.comments[id] = [];
      }
      this.comments[id].push(comment);
      console.log("Comments: ", this.comments); 
    },

    filterProfiles() {
      const query = this.searchQuery.toLowerCase();
      this.filteredProfiles = this.profiles.filter(profile =>
        profile.name.toLowerCase().includes(query) ||
        profile.email.toLowerCase().includes(query) ||
        profile.description.toLowerCase().includes(query)
      );
    },

    addProfile() {
      const newId = this.profiles.length ? this.profiles[this.profiles.length - 1].id + 1 : 1;
      const newProfile = { ...this.newProfile, id: newId, likes: 0 };
      this.profiles.push(newProfile);
      this.newProfile.name = "";
      this.newProfile.email = "";
      this.newProfile.description = "";
      this.filterProfiles();
    },
  },
};
</script>

<style>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.section {
  margin: 20px;
}

.flex-row {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.label {
  margin-right: 10px;
}

.input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.buttons {
  margin-bottom: 20px;
}

.profile {
  margin-bottom: 20px;
}

.header {
  font-size: 1.5em;
  margin-bottom: 20px;
}

button {
  padding: 10px 20px;
  background-color: #42b983;
  border: none;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #339966;
}
</style>
