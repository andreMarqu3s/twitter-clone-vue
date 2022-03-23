<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="!user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
      <form class="user-profile__create-tweet" @submit.prevent="createNewTweet">
        <label for="newTweet"><strong>New tweet</strong></label>
        <textarea id="newTweet" rows="4" v-model="newTweetContent" />
        <div class="user-profile__create-tweet-type">
          <label for="newTweetType"><strong>Type: </strong></label>
          <select id="newTweetType" v-model="selectedTweetType">
            <option
              :value="option.value"
              v-for="(option, index) in tweetTypes"
              :key="index"
            >
              {{ option.name }}
            </option>
          </select>
        </div>
        <button>Tweet</button>
      </form>
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem
        class="user-profile__tweet"
        v-for="tweet in user.tweets"
        :key="tweet.id"
        :username="user.username"
        :tweet="tweet"
        @favourite="toggleFav"
      />
    </div>
  </div>
</template>

<script>
import TweetItem from "./TweetItem";

export default {
  name: "UserProfile",
  components: { TweetItem },
  data() {
    return {
      newTweetContent: "",
      selectedTweetType: "instant",
      tweetTypes: [
        {
          value: "draft",
          name: "Draft",
        },
        {
          value: "instant",
          name: "Instant Tweet",
        },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "_pepesl",
        firstName: "Pepe",
        lastName: "Silva",
        email: "pepeslv@email.com",
        isAdmin: true,
        tweets: [
          {
            id: 1,
            content: "first tweet",
          },
          {
            id: 2,
            content: "second tweet",
          },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerAccount, oldFollowerAccount) {
      if (oldFollowerAccount < newFollowerAccount) {
        console.log(`${this.user.username} has gained a follower`);
      }
    },
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFav(id) {
      console.log(`fav ${id}`);
    },
    createNewTweet() {
      if (this.newTweetContent && this.selectedTweetType !== "draft") {
        this.user.tweets.unshift({
          id: this.user.tweets.length + 1,
          content: this.newTweetContent,
        });
        this.newTweetContent = "";
      }
    },
  },
  mounted() {
    this.followUser();
  },
};
</script>

<style scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
  background: rebeccapurple;
  color: white;
  border-radius: 5px;
  margin-right: auto;
  padding: 0 10px;
  font-weight: bold;
}

.user-profile__tweets-wrapper {
  display: grid;
  grid-gap: 10px;
  margin-bottom: auto;
}

.user-profile__create-tweet {
  display: flex;
  flex-direction: column;
  border: 1px solid dfe3e8;
  padding-top: 20px;
}

h1 {
  margin: 0;
}
</style>