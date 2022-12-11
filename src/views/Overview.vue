<template>
  <main>
    <!-- <Sidebar style="position: fixed;" /> -->
    <div class="dashboard__container">
      <div class="dashboard__container--navigation">
        <input type="text" name="search" id="" placeholder="Search" class="navigation--searchInput">
        <div class="navigation__notifications">
          <button class="navigation__notifications--button navigation__notifications--notifyButton"></button>
          <button class="navigation__notifications--button navigation__notifications--hamburgerButton"></button>
        </div>
      </div>
      <div class="dashboard__container--balance">
        <CurrentBalance />
      </div>
      <div class="dashboard__container--summary">
        <Summary />
      </div>
      <div class="dashboard__container--stats">
        <CryptoStatistics />
      </div>
    </div>
  </main>
</template>

<script>
// @ is an alias to /src
// vimport Sidebar from '@/components/Sidebar.vue'
import CurrentBalance from '@/components/CurrentBalance.vue'
import CryptoStatistics from '@/components/CryptoStatistics.vue'
import Summary from '@/components/Summary.vue'

export default {
  name: 'Home',
  components: {
    // Sidebar,
    CurrentBalance,
    CryptoStatistics,
    Summary
  }
}
</script>


<style scoped>
.dashboard__container {
  min-height: 100vh;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: auto;
  grid-row-gap: 10px;
}

.dashboard__container--navigation {
  height: 50px;
  grid-area: 1 / 2 / 2 / 5;
  margin-top: 50px;
  display: flex;
}

.navigation__notifications--button {
  width: 48px;
  height: 48px;
  margin-left: 15px;
  border-radius: 8px;
  background: var(--light-gray-background);
  cursor: pointer;
}

.navigation__notifications--notifyButton {
  background-image: url('../assets/icons/notify.svg');
  background-repeat: no-repeat;
  background-position: center;
}

.navigation__notifications--hamburgerButton {
  background-image: url('../assets/icons/hamburger.svg');
  background-repeat: no-repeat;
  background-position: center;
}


.navigation__notifications {
  margin-left: auto;
}

.dashboard__container--balance {
  width: 509px;
  grid-area: 2 / 2 / 2 / 4;
}

.dashboard__container--summary {
  width: 499px;
  grid-area: 2 / 4 / 4 / 5;
}

.dashboard__container--stats {
  grid-area: 3 / 2 / 6 / 5;
}

.navigation--searchInput {
  border: none;
  outline: none;
  border-radius: 8px;
  width: 232px;
  height: 48px;
  background: var(--light-gray-background);
  color: var(--dark-gray-text);

  background-image: url('../assets/icons/search.svg');
  background-repeat: no-repeat;
  background-position: 5% 50%;
}

.navigation--searchInput::placeholder {
  padding-left: 5px;
}

.navigation--searchInput[type="text"] {
  padding-left: 32px;
}

@media only screen and (max-width: 800px) {
  .dashboard__container {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    flex-direction: column;
    gap: 0;
  }

  .dashboard__container--navigation {
    margin-bottom: 10px;
  }

  .dashboard__container--stats {
    width: 509px;
    margin-top: 10px;
  }

  .dashboard__container--summary {
    margin-top: 10px;
    order: 3;
  }
}
</style>
