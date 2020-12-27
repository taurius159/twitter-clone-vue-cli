<template>
<div class="container">
  <div class="row justify-content-start">
    <div class="col-md-3">
      <div class="user-profile">
        <div class="user-profile_user-panel">
          <h1 class="user-profile_username">@{{user.userName}}</h1>
          <div class="user-profile_admin-badge" v-if=user.isAdmin>
            Admin </div>
          <div class="user-profile_follower-count" >
            <strong> Followers: </strong> {{followers}}
        </div>
      </div>
  </div>
    </div>

      <div class="col-md-6">
        <form class="user-profile_create-tweet" @submit.prevent="createNewTweet" :class="{ '--exceeded': newTweetCharacterCount > 180 }">
          <label for="newTweet"><strong>New Tweet</strong>({{newTweetCharacterCount}}/180)</label>
          <textarea id="newTweetContentId" rows="4" v-model="newTweetContent"></textarea>
          <div class="user-profile_create-tweet-type">
            <label for="newTweetTypeId"><strong>Type:</strong></label>
            <select id="newTweetTypeId" v-model="newTweetType">
              <!--<option :value="option.value" v-for="option in tweetTypes" :key="option.value" > -->
              <option :value="option.value" v-for="(option, index) in tweetTypes" :key="index" > 
              {{option.name}}
              </option>
            </select>
          </div>

          <button>
            Tweet!
          </button>

        </form>
    </div>
  </div>
  <div class="row justify-content-center">
    <div class="col-md-6">
      <div class="user-profile_tweets-wrapper">
          <TweetItem 
            v-for="tweet in user.tweets" 
            :key=tweet.id 
            :userName="user.userName" 
            :tweet="tweet" 
            @favourite="toggleFavourite">
          </TweetItem>
        </div>
    </div>
  </div>
</div>
</template>

<script>
import TweetItem from "./TweetItem.vue";

export default 
  {
  name: 'UserProfile',
  components: {TweetItem},
  data() {
    return {
      changes: 'changes',
      newTweetContent: '',
      newTweetType: 'instant',
      tweetTypes: [
        {value: 'draft', name: 'Draft'},
        {value: 'instant', name: 'Instant Tweet'}
      ],
      followers: 0,
      user: {
        id: 0,
        userName: 'MichaelScott',
        firstName: 'Michael',
        lastName: 'Scott Junior',
        email: 'RyanBurnedOffice@microware.com',
        isAdmin: false,
        tweets: [
          {id: 0, content: 'Twitter is amazing!'},
          {id: 1, content: 'Michael Scott company'}
        ]
      }
    }
    },
    watch: {
      followers(newFollowerCount, oldFollowerCount){
        if(oldFollowerCount < newFollowerCount){
          console.log('@' + this.user.userName + ' has gained a new follower!')
        }
      }
    },
      computed: {
        fullName(){
          return this.user.firstName + ' ' + this.user.lastName;
        },
        newTweetCharacterCount(){
          return this.newTweetContent.length;
        } 
      },
      methods: {
        createNewTweet()
        {
          if(this.newTweetContent && this.newTweetType !== 'draft')
          {
            this.user.tweets.unshift(
              {
                id: this.user.tweets.length+1,
                content: this.newTweetContent
              }
            )
          }
          this.newTweetContent = ''
        },
        followUser()
        {
          this.followers++;
        },
        toggleFavourite(id)
        {
          console.log('Favourited tweet #'+id)
        }
      },
      mounted() {
        this.followUser();
      }
    }
</script>

<style lang="scss" scoped>
.row{
  padding: 10px 10px 10px 10px;
  margin-block: 5px;
}
.user-profile{
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: auto;

.user-profile_user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #DFE3E8
}
.user-profile_admin-badge {
  background: purple;
  color: white;
  border-radius: 10px;
  margin: auto;
  padding: 0 10px;
  font-weight: bold;
}
h1 {
  margin: 0;
}
}
.user-profile_create-tweet{
  padding-top:20px;
  display: grid;
  border-radius: 1px solid #DFE3E8;
  &.--exceeded{
  color:red;
  border-color: red;
  button{
    border: none;
    background-color: red;
    color: white;
  }
}
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
