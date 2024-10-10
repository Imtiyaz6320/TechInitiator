<template>
  <div class="wizfit-challenge">
    <header class="header">
      <img src="../assets/banner.png" alt="WizFit Challenge Banner" class="banner" />
    </header>

    <section class="info-section">
      <h2>What is WizFit Challenge?</h2>
      <button @click="watchVideo" class="watch-button">Watch Video</button>
    </section>

    <img src="../assets/player.png" alt="Player Image" class="player-image" />

    <section class="challenge-signup">
      <div class="signup-box">
        <h2>Are you ready to take the challenge?</h2>
        <p>Download Harlem Wizards App</p>
        <div class="app-links">
          <a href="#" class="app-link">
            <img src="../assets/goolge-store.png" alt="Google Play Store" class="app-image" />
          </a>
          <a href="#" class="app-link">
            <img src="../assets/apple-store.png" alt="Apple Store" class="app-image" />
          </a>
        </div>

        <p>Search for your school:</p>
        <input v-model="searchQuery" @input="fetchSchools" type="text" placeholder="Search Campaign..." class="input" />

        <div class="school-list">
          <ul>
            <li v-if="filteredSchools.length === 0 && searchQuery.length > 0">No schools found.</li>
            <li v-for="school in filteredSchools" :key="school.id" class="school-item">
              {{ school.user_name }}
              <button class="join-button" @click="joinCampaign(school.user_name)">Join Campaign</button>
            </li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>


<script>
export default {
  data() {
    return {
      schools: [],      // List of schools
      searchQuery: '',  // Search input from user
      loading: false,   // Loading state
      errorMessage: '', // Error message state
    };
  },
  computed: {
    filteredSchools() {
      // Filter schools 
      return this.schools.filter(school =>
        school.user_name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
  },
  methods: {
    async fetchSchools() {
      this.loading = true; // Show loading 
      this.errorMessage = ''; //error message

      const url = `http://api.devharlemwizardsinabox.com/campaign/campaign_school_list/?search=${encodeURIComponent(this.searchQuery)}`;

      try {
        const response = await fetch(url, {
          method: 'GET',
          headers: {
            'Content-Type': 'application/json',
          },
        });

        if (!response.ok) {
          throw new Error(`Network response was not ok: ${response.statusText}`);
        }

        const data = await response.json();
        // console.log('API Response:', data);
        this.schools = Array.isArray(data.school_list) ? data.school_list : [];

      } catch (error) {
        console.error('Error fetching schools:', error);
        this.errorMessage = 'Failed to load schools. Please try again later.';
        this.schools = [];
      } finally {
        this.loading = false; // Hide loading indicator
      }
    },
    joinCampaign(schoolName) {
      alert(`Joining campaign for ${schoolName}`);
    },
    watchVideo() {
      alert("Video played!");
    },
  },
  mounted() {
    this.fetchSchools();
  },
};
</script>

<style scoped>
.wizfit-challenge {
  font-family: Arial, sans-serif;
  text-align: center;
  padding: 20px;
  background-color: #f9f9f9;
}

.banner {
  width: 100%;
  max-height: 300px;
  object-fit: cover;
}

.header {
  background-color: #6a0dad;
  color: white;
  padding: 20px;
}

.info-section {
  margin: 20px 0;
  color: #ff1493;
}

.watch-button {
  background-color: #ff1493;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.challenge-signup {
  margin-top: 30px;
  color: #ff1493;
}

.signup-box {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  padding: 20px;
  margin: 0 auto;
  max-width: 600px;
}

.app-links {
  margin: 20px 0;
  display: flex;
  justify-content: center;
}

.app-image {
  width: 150px;
  margin: 0 10px;
}

.input {
  padding: 10px;
  margin: 10px 0;
  width: 80%;
  max-width: 300px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.school-list {
  margin-top: 20px;
  text-align: left;
}

.school-item {
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

  .join-button {
    background-color: #32cd32;
    color: white;
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .player-image {
    width: 200px;
    margin: 20px 0;
  }</style>