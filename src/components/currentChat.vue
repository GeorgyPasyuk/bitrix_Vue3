<template>
  <div class="main">
    <div class="header">
      <div class="messageAvatar"></div>
      <div class="headerInfo">
        <div class="headerFullname">
          <div class="headerName">{{ items[currentIndex].account_name }}</div>
          <div class="headerTime">
            Заходил сегодня в {{ latestTime[currentIndex] }}
          </div>
        </div>
        <div class="headerBg">пользователь</div>
      </div>
      <div class="headerButtons">
        <button class="callBtn">Видеозвонок HD</button>
        <button class="inviteBtn">+ Пригласить</button>
        <button class="findBtn">
          <img src="./images/icons/lupa.svg" alt="" />
        </button>
      </div>
    </div>
    <div class="messageContainer">
      <div class="previousContainer">
        <span class="previousMsg">Загрузить более ранние сообщения</span>
        <a href="#" class="previousDate">сегодня</a>
      </div>

      <div
        class="messageItems"
        v-for="(item, index) in sentMessages.message"
        :key="index"
      >
        <div class="messageAvatar"></div>
        <div class="sentMessage">
          {{ item }}
          <div class="infoContainer">
            <div class="likes">
              {{ sentMessages.likes[index] }}
              <img
                @click="liked"
                src="./images/like.svg"
                alt=""
                class="likedMsg"
              />
            </div>
            <div class="currentTime">
              Нравится {{ sentMessages.time[index] }}
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="inputContainer">
      <textarea
        name=""
        id="textarea"
        cols="30"
        rows="10"
        class="textInput"
        @keydown.enter="getMessage"
        v-model="input"
        placeholder="Введите сообщение"
      ></textarea>
      <div class="icons">
        <img src="./images/icons/addFiles.svg" alt="" />
        <img src="./images/icons/person.svg" alt="" />
        <img src="./images/icons/square.svg" alt="" />
        <img src="./images/icons/smile.svg" alt="" />
        <img src="./images/icons/double.svg" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "currentChat",
  props: ["items", "currentIndex", "latestTime"],
  data() {
    return {
      input: "",
      sentMessages: { message: [], time: [], likes: [] },
      formattedMessages: "",
    };
  },
  mounted() {
    const storageObject = JSON.parse(
      localStorage.getItem(`${this.currentIndex}`)
    );
    if (storageObject !== null) {
      this.sentMessages.message = storageObject.message;
      this.sentMessages.time = storageObject.time;
      this.sentMessages.likes = storageObject.likes;
    }
  },
  methods: {
    sendEmit(currentTime) {
      const latestMessage = {
        index: this.currentIndex,
        message: this.input,
        time: currentTime,
      };
      this.$emit("customChange", latestMessage);
    },
    splitMessage() {
      this.formattedMessages = "";
      let i = 0;
      while (i < this.input.length) {
        const next = this.input.substr(i, 90);
        this.formattedMessages += `${next}\n`;
        i += 90;
      }
    },
    getTime() {
      const date = new Date();
      const hours = date.getHours();
      const minutes = date.getMinutes();

      const formattedHours = hours < 10 ? `0${hours}` : `${hours}`;
      const formattedMinutes = minutes < 10 ? `0${minutes}` : `${minutes}`;

      const currentTime = `${formattedHours}:${formattedMinutes}`;

      this.sentMessages.time.push(currentTime);
      this.sendEmit(currentTime);
    },

    liked() {
      this.sentMessages.likes.push(1);
    },
    storageManagerSet() {
      localStorage.setItem(
        `${this.currentIndex}`,
        JSON.stringify(this.sentMessages)
      );
    },
    getMessage() {
      this.splitMessage();
      this.sentMessages.message.push(this.formattedMessages);
      this.getTime();
      this.storageManagerSet();
      this.input = "";
    },
  },
};
</script>

<style>
@import "chatStyles.scss";
</style>
