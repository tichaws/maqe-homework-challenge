<template>
  <div class="container">
    <div class="title">
      <h1>MAQE Forum</h1>
      <span>Your currently timezone is : {{ timeZone }}</span>
    </div>
    <div class="card-container">
      <div v-for="item in combinedData" :key="item.id" class="card">
        <div v-if="item.author" class="card-header">
          <img :src="item.author.avatar_url" :alt="item.author.name" class="profile-image" />
          <div class="info">
            <span class="name">{{ item.author.name }}</span>
            <span v-if="item.content.created_at" class="date"
              >Posted on
              {{
                new Intl.DateTimeFormat("en-US", this.dateFormat).format(
                  new Date(item.content.created_at)
                )
              }}</span
            >
          </div>
        </div>
        <div v-if="item.author" class="divider" />
        <div class="card-content">
          <div class="left-column" v-if="item.content.image_url">
            <img
              :src="item.content.image_url"
              :alt="item.content.title"
              class="content-image"
            />
          </div>
          <div class="right-column">
            <h2 v-if="item.content.title" class="topic">{{ item.content.title }}</h2>
            <span class="description">{{ item.content.body }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeZone: "",
      posts: [],
      authors: [],
      //   combinedData: [],
      dateFormat: {
        weekday: "long", // Abbreviated day name, e.g., "Wed"
        year: "numeric",
        month: "long", // Full month name, e.g., "April"
        day: "numeric",
        hour: "2-digit",
        minute: "2-digit",
        hour12: false, // 24-hour format
      },
    };
  },
  async mounted() {
    this.timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
    this.posts = await this.fetchURL("/posts.json");
    this.authors = await this.fetchURL("/authors.json");
  },
  methods: {
    async fetchURL(url) {
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        const data = await response.json();
        return data;
      } catch (error) {
        console.error("Error fetching JSON:", error);
        return null; 
      }
    }
  },
  computed: {
    combinedData() {
      if (!this.posts || !this.authors) return [];
      return this.posts
        .map((post) => {
          const authorContent = this.authors.find((item) => post.id === item.id);
          return {
            author: authorContent ? authorContent : undefined,
            content: post,
          };
        })
        .filter((item) => item.author !== undefined)
        .sort((a, b) => new Date(b.content.created_at) - new Date(a.content.created_at));
    },
  },
};
</script>

<style scoped>
.container {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-direction: column;
}
.title {
  width: 100%;
  max-width: 60rem;
  text-align: left;
}
.card-container {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-direction: column;
  width: 100%;
  max-width: 60rem;
}

.card {
  width: 100%;
  border: 1px solid #e0e0e0;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  background-color: white;
}

.card-container .card:nth-child(even) {
  background-color: #ccecff;
}

.card-header {
  display: flex;
  align-items: center;
  padding: 0px 16px 0px 16px;
  margin: 4px 0px 4px 0px;
}

.card-container .card:nth-child(even) .divider {
  background-color: #c9c9c9;
}

.divider {
  width: 100%;
  height: 1px;
  background-color: #dddddd;
  margin-bottom: 12px;
}

.card-content {
  display: flex;
  gap: 16px;
  padding: 16px;
  font-size: 1rem;
  color: #333;
}

.profile-image {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-right: 12px;
  object-fit: cover;
}

.info {
  display: flex;
  flex-direction: row;
  gap: 5px;
}

.name {
  font-weight: bold;
  color: orange;
  font-size: small;
}

.date {
  font-size: small;
  color: #aeacac;
  font-weight: bold;
}

.left-column {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.content-image {
  width: 100%;
  object-fit: cover;
}

.right-column {
  flex: 2;
  display: flex;
  flex-direction: column;
  justify-content: start;
  text-align: left;
}

.topic {
  font-size: 1.5rem;
  font-weight: bold;
  color: #333;
  margin: 0 0 8px;
}

.description {
  font-size: 1rem;
  color: #666;
}
</style>
