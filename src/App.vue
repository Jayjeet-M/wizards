<template>
  <div id="app">
    <!-- Full-width header section -->
    <header class="header-section">
      <img src="@/assets/header-image.png" alt="WizFit Challenge" class="header-image" />
    </header>
    <section class="challenge-info">
      <h2 style="color: red; ">What is WizFit Challenge?</h2>
      <button class="watch-video-btn"
        style="display: flex; align-items: center; justify-content: center; border: none; background-color: #ff0000; color: white; padding: 10px 20px; border-radius: 5px; cursor: pointer;"
        @click="openVideo">
        Watch Video
        <img src="@/assets/image.png" alt="Play Icon" style="height: 40px; width: 40px; margin-left: 8px;">
      </button>
    </section>

    <section class="form-section">
      <div class="form-container">
        <div class="man-image">
          <img src="@/assets/man.png" alt="WizFit Man" class="man-cutout" />
        </div>

        <div class="form-content">
          <h3 style="color: red;">Are you ready to take the challenge?</h3>
          <h5>Download Harlem Wizards App</h5>
          <div class="download-icons">
            <a href="https://play.google.com/store/apps" target="_blank" rel="noopener noreferrer">
              <img src="@/assets/google-play.png" alt="Google Play" class="app-icon" />
            </a>
            <a href="https://apps.apple.com/us/app/app-store/id1234567890" target="_blank" rel="noopener noreferrer">
              <img src="@/assets/app-store.png" alt="App Store" class="app-icon" />
            </a>
          </div>
          <div style="display: flex; align-items: center; justify-content: center;">
            <hr
              style="flex: 1; border: none; height: 1px; background-color: #ccc; margin-right: 20px; margin-left:100px" />
            <p style="margin: 0 10px;">or you can signup right now</p>
            <hr
              style="flex: 1; border: none; height: 1px; background-color: #ccc; margin-left: 20px;margin-right: 100px;" />
          </div>
          <div style="width: 400px; margin: 0 auto; ">
            <input type="text" v-model="searchQuery" placeholder="Search Campaigns..." @input="fetchSchools" />

            <ul class="school-list">
              <li v-for="school in filteredSchools.slice(0, 2)" :key="school.id">
                <span>{{ school.school_name }}</span>
                <button class="join-btn" @click="join">Join Campaign</button>
              </li>
            </ul>
            <p v-if="loading">Loading Campaigns...</p>
            <p v-if="filteredSchools.length === 0 && !loading">No campaigns found!</p>
            <p v-if="error">Something went wrong, please retry</p>
          </div>


        </div>
      </div>
    </section>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      schools: [],
      searchQuery: '',
      loading: false,
      error: false
    };
  },
  methods: {
    async fetchSchools() {
      try {
        this.loading = true
        const response = await axios.get(`http://api.devharlemwizardsinabox.com/campaign/campaign_school_list/?search=${this.searchQuery}`);

        this.schools = response.data.school_list;
      } catch (error) {
        console.error('Error fetching schools:', error);
        this.error = true
      }
      finally {
        this.loading = false
      }
    },
    openVideo() {
      window.open('https://www.youtube.com/', '_blank'); // Replace YOUR_VIDEO_ID with the actual ID
    },
    join() {
      console.log(`called`)
      alert('Joined Successfully');
    },
  },

  computed: {
    filteredSchools() {
      return this.schools.filter((school) =>
        school?.school_name?.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    }
  },
  mounted() {
    this.fetchSchools();
  }
};
</script>

<style scoped>
* {
  font-family: 'Poppins', sans-serif !important;
}

/* Header Section */
.header-section {
  position: relative;
  width: 100%;
  height: 250px;
}

.header-image {
  width: 100%;
  height: 100%;
  object-fit: fill;
  position: absolute;
  top: 0;
  left: 0;
}

.header-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  text-align: center;
}

.watch-video-btn {
  background-color: #ff005d;
  color: white;
  padding: 2px 2px;
  border: none;
  border-radius: 10px;
  font-size: 1em;
  cursor: pointer;
  margin-left: 10px;
}

.challenge-info {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  margin-top: 10px;
  margin-right: 10px
}

/* Main Form Section */
.form-section {
  position: relative;
  text-align: center;
  margin-top: 90px;
  z-index: 999;
}

.form-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
}

.form-content {
  background-color: white;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  max-width: 600px;
  margin-top: 100px;
  position: relative;
  z-index: 2;
  width: 600px;
}

.man-image {
  position: absolute;
  top: -80px;
  z-index: 0;
}

.man-cutout {
  width: 250px;
  height: auto;
}

.download-icons {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.app-icon {
  width: 150px;
  height: auto;
  margin: 0 10px;
}

input[type="text"] {
  width: 90%;
  max-width: 400px;
  padding: 10px;
  margin-top: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

/* Styling for Search Results */
.school-list {
  list-style-type: none;
  padding: 0;
  margin-top: 20px;
}

.school-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f9f9f9;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}

.join-btn {
  background-color: #ff0000;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.join-btn:hover {
  background-color: #cc0000;
}
</style>
