<template>
    <div id="post">
        <div>
            <div class="user">
                <img :src="userPhoto">
                <h1> {{ firstName }} {{ lastName }} </h1>
            </div>
            <div class="date">
                <p>{{ createdAt | moment("ddd, hA") }}</p>
            </div>
        </div>
        <div class="content">
            <p> {{ content }}</p>
            <img :src="postPhoto">
        </div>
        <!-- Dernier com -->
        <slot name="Comments"></slot>
        <slot name="lastCommentZone"></slot>
    </div>
</template>

<script>
export default {
    name: 'Post',
    data() {
        return {
        }
    },
    props: {
        firstName: {
            type: String,
            default: ""
        },
        lastName: {
            type: String,
            default: ""
        },
        userPhoto: {
            type: String,
            default: ""
        },
        createdAt: {
            type: String,
            default: "2 heures"
        },
        content: {
            type: String,
            default: "This is my first message !"
        },
        myUserPhoto: {
            type: String,
            default:""
        },
        postId: {
            type: String
        },
        postPhoto: {
            type: String
        },
        newCommentContent: {
            type: String
        }
    },
}
</script>

<style>
    #post {
        margin-top: 50px;
        border-radius: 10px;
        box-shadow: 5px 5px 5px #b1a7a6;
        width: 600px;
        margin-left: 650px;
        background-color: #ffffff;
    }
    .user {
        display: flex;
        padding-top: 20px;
    }
    .user img {
        border-radius: 50px;
        max-width: 60px;
        max-height: 60px;
        height: 100%;
        width: 100%;
        margin-left: 10px;
    }
    .user h1 {
        padding: 10px 15px 0 15px;
        font-size: 1.5rem;
    }
    .date {
        text-align: end;
        margin-right: 10px;
    }
    .content {
        margin-top: 30px;
        text-align: center;
    }
    .content p {
        font-size: 1.3rem;
        margin-bottom: 20px;
    }
    .content img {
        box-shadow: 5px 5px 5px rgb(212, 212, 212);
        max-width: 500px;
        max-height: 500px;
        width: 100%;
        height: 100%;
    }

</style>