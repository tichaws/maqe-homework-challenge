<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <div class="container">
      <div class="line-container">
        <div class="line"></div>
        <div v-for="(item, index) in text.split('')" :key="item.id">
          <div class="circle">
            <span v-bind:class="(index === position) ? 'rotate':''">{{item}}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "StylesComponent",
  props: {
    title: String,
    description: String,
  },
  data() {
    return {
      text: "MAQE",
      position: 2,
    };
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  display: flex;
  justify-content: center;
  overflow-x: auto;
}

.line-container {
  display: flex;
  align-items: center;
  gap: 0.1rem; 
  position: relative;
  width: 55rem;
  padding: 1rem;
}

.line-container::before {
  content: "";
  position: absolute;
  top: 47%;
  left: 0;
  right: 0;
  height: 1rem;
  background-color: #3a4454;
  z-index: -1; 
  border-radius: 0.25rem;
  box-shadow: 2px 2px 15px rgb(0 0 0 / 80%);
}

.circle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 10rem;
  height: 10rem;
  background: linear-gradient(160deg, #70c6e4, #9cd5d2);
  border-radius: 50%;
  font-weight: bold;
  color: black;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  position: relative;
}

.circle::before {
  content: "";
  position: absolute;
  inset: 0.5rem; /* Creates inner border effect */
  border-radius: 50%;
  border: 0.5rem solid #fff; /* Inner border color */
  box-sizing: border-box;
}

.circle span {
  text-shadow: 0px 0px 6px rgba(0, 0, 0, 0.7); /* Adds a shadow to the text */
  font-size: 4rem;
  font-weight: bold;
  color: white;

  &.rotate {
    transform: rotate(240deg);
  }
}

/* Adjustments for Small Screens */
@media (max-width: 768px) {
  .main {
    overflow-x: auto; /* Allow horizontal scroll */
  }

  .line-container {
    gap: 0.1rem; /* Reduce gap between circles */
  }

  .circle {
    width: 7rem; /* Smaller circles for mobile */
    height: 7rem;
  }

  .circle span {
    font-size: 3rem; /* Adjust font size for smaller circles */
  }
}

@media (max-width: 480px) {
  .circle {
    width: 5rem; /* Smaller circles for very small screens */
    height: 5rem;
  }

  .circle span {
    font-size: 2rem;
  }
}
</style>
