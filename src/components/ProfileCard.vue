<template>
  <div class="profile">
    <div class="card">
      <DoctorIcon class="avatar" />
      <div class="data">
        <div class="profile-content">
          <div>
            <strong>{{ profile.name }}</strong>
            <a :href="profile.email" class="email">{{ profile.email }}</a>
          </div>
          <div class="description">{{ profile.description }}</div>
        </div>
        <div class="likes">
          <span class="likes-icon">&#10084;</span>
          <span class="likes-value">{{ profile.likes }}</span>
        </div>
      </div>
    </div>
    <div class="comment">
      <input
        class="comment-input"
        v-model="comment"
        placeholder="Write your comment..."
        @change="handleComment"
      />
    </div>
  </div>
</template>

<script>
import DoctorIcon from "./DoctorIcon.vue";

export default {
  name: "ProfileCard",

  components: {
    DoctorIcon,
  },

  props: {
    profile: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      comment: "",
    };
  },

  methods: {
    handleComment() {
      this.$emit("add-comment", { id: this.profile.id, comment: this.comment });
      this.comment = "";
    },
  },
};
</script>

<style>
.profile {
  padding: 20px;
}

.card {
  display: flex;
  align-items: center;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.avatar {
  width: 50px;
  height: 50px;
  margin-right: 20px;
}

.data {
  display: flex;
  flex-direction: column;
}

.profile-content {
  margin-bottom: 10px;
}

.email {
  color: blue;
  text-decoration: underline;
}

.likes {
  display: flex;
  align-items: center;
}

.likes-icon {
  margin-right: 5px;
  color: red;
}

.comment {
  margin-top: 10px;
}

.comment-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
