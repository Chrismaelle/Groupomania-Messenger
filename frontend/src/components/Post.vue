<template>
    <div class="post c-6 cm-12">
        <div class="top-post">
            <div class="c-8 cs-10 user-id">
                <img id="user-photo" :src="userPhoto">
                <p class="user-name"> {{ firstName }} {{ lastName }} </p>
            </div>
            <div class="c-4 cs-2">
                <p class="post-date">{{ createdAt | moment("ddd, hA") }}</p>
            </div>
        </div>
        <div class="content">
            <p class="message-post"> {{ content }}</p>
            <img :src="postPhoto" class="media-post">
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