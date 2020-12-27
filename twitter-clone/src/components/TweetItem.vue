<template>
<div class="tweet-item" @click="favoriteTweet(tweet.id)">
    <div class="user-profile_tweet">
        <div class="tweet-item_user">
            @{{ userName }}
        </div>
        <div class="tweet-item_content">
            {{tweet.content}}
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: "TweetItem",
    props: {
        userName: {
            type: String,
            required: true
        },
        tweet: {
            type: Object,
            required: true
        }
    },
    methods: {
    favoriteTweet(id)
    {
        this.$emit('favourite', id);
    }
    }
};
</script>
<style scoped>
.tweet-item{
display: right;
padding: 10px;
background-color:white;
border-radius: 10px;
border: 1px solid #DFE3E8;
box-sizing: border-box;
cursor: pointer;
transition: all 0.25s ease;
}

.tweet-item:hover{
    transform: scale(1.1, 1.1);
}

.tweet-item_user{
font-weight: bold;
}

</style>