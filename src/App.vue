<template>
  <div class="window">
    <div class="main_nav_bar"></div>
    <div class="msg_box">
      <div class="msg_nav_bar">
        <div class="icons_items">
          <img src="./images/icons/msg.svg" alt="#" class="msg_icons" />
          <img src="./images/icons/lines.svg" alt="#" class="msg_icons" />
          <img
            src="./images/icons/notifications.svg"
            alt="#"
            class="msg_icons"
          />
          <img src="./images/icons/calls.svg" alt="#" class="msg_icons" />
          <img
            src="./images/icons/settings.svg"
            alt="#"
            class="msg_icons settingsIcon"
          />
          <img src="./images/icons/news.svg" alt="#" class="msg_icons" />
        </div>
      </div>
      <div class="chats">
        <div class="search">
          <span class="lupa" @click="getChildVariable"></span>
          <button class="createDialogue">Создать</button>
          <span class="cross" @click="clearInput" v-if="inputModel"></span>
          <input
            class="input_search"
            id="searchInput"
            placeholder="Поиск"
            type="text"
            v-model="inputModel"
          />
        </div>
        <div class="chats_container">
          <div
            class="chat_item"
            @click="sendChatId(index)"
            v-for="(item, index) in items"
            v-bind:key="item.account_id"
          >
            <div class="avatar_box">
              <div class="avatar" :style="avatarCol[index]">
                {{ avatarLetters[index] }}
              </div>
            </div>
            <div class="chat_info">
              <div class="name">{{ item.account_name }}</div>
              <div class="latest_message">
                <img
                  src="./images/icons/reply.svg"
                  alt="#"
                  class="itemReply"
                />{{ latestEvents.latestMessage[index] }}
              </div>
              <div class="latest_time">
                <img src="./images/icons/read.svg" alt="#" class="itemRead" />
                {{ latestEvents.latestTime[index] }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="empty_dialogue">
        <current-chat
          v-if="this.currentIndex === 0"
          :items="items"
          :currentIndex="currentIndex"
          :latestTime="latestEvents.latestTime"
          @customChange="logChange"
        />
        <current-chat
          v-if="this.currentIndex === 1"
          :items="items"
          :currentIndex="currentIndex"
          :latestTime="latestEvents.latestTime"
          @customChange="logChange"
        />
        <current-chat
          v-if="this.currentIndex === 2"
          :items="items"
          :currentIndex="currentIndex"
          :latestTime="latestEvents.latestTime"
          @customChange="logChange"
        />
        <current-chat
          v-if="this.currentIndex === 3"
          :items="items"
          :currentIndex="currentIndex"
          :latestTime="latestEvents.latestTime"
          @customChange="logChange"
        />
        <current-chat
          v-if="this.currentIndex === 4"
          :items="items"
          :currentIndex="currentIndex"
          :latestTime="latestEvents.latestTime"
          @customChange="logChange"
        />
        <div class="empty_dialogue_text" v-if="this.currentIndex === ''">
          Начните диалог, выбрав контакт из списка слева.
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import currentChat from "@/components/currentChat";

export default {
  name: "App",
  props: ["sentMessages"],
  data() {
    return {
      items: [
        {
          account_id: "1",
          account_name: "Максим Соловьев",
        },
        {
          account_id: "2",
          account_name: "Павел Солнцев",
        },
        {
          account_id: "3",
          account_name: "Иван Куштейко",
        },
        {
          account_id: "4",
          account_name: "Jack Williams",
        },
        {
          account_id: "5",
          account_name: "Тамерлан Аскеров",
        },
      ],
      currentIndex: "",
      latestEvents: {
        latestMessage: {
          0: "",
          1: "",
          2: "",
          3: "",
          4: "",
        },
        latestTime: {
          0: "",
          1: "",
          2: "",
          3: "",
          4: "",
        },
      },
      inputModel: "",
      avatarCol: {
        0: {
          backgroundColor: "",
        },
        1: {
          backgroundColor: "",
        },
        2: {
          backgroundColor: "",
        },
        3: {
          backgroundColor: "",
        },
        4: {
          backgroundColor: "",
        },
      },
      avatarLetters: {
        0: "",
        1: "",
        2: "",
        3: "",
        4: "",
      },
    };
  },
  mounted() {
    let storage = JSON.parse(localStorage.getItem("latestEvents"));
    for (let i = 0; i < Object.keys(storage.latestMessage).length; i++) {
      this.latestEvents.latestMessage[i] = storage.latestMessage[i];
      this.latestEvents.latestTime[i] = storage.latestTime[i];
    }
  },
  created() {
    for (let i = 0; i < 5; i++) {
      this.avatarCol[i].backgroundColor =
        "#" + Math.floor(Math.random() * 16777215).toString(16);
    }
    for (let i = 0; i < this.items.length; i++) {
      let str = this.items[i].account_name;
      const nameSplit = str.split(" ");
      const firstNameLetter = nameSplit[0][0];
      const lastNameLetter = nameSplit[1][0];
      this.avatarLetters[i] = `${firstNameLetter}${lastNameLetter}`;
    }
  },
  components: { currentChat },
  methods: {
    clearInput() {
      const input = document.getElementById("searchInput");
      return (input.value = "");
    },
    sendChatId(index) {
      this.currentIndex = index;
    },
    logChange(sentMessage) {
      if (sentMessage.message.length >= 90) {
        this.latestEvents.latestMessage[sentMessage.index] =
          sentMessage.message.slice(0, 20) + "...";
        this.latestEvents.latestTime[sentMessage.index] = sentMessage.time;
      } else {
        this.latestEvents.latestMessage[sentMessage.index] =
          sentMessage.message;
        this.latestEvents.latestTime[sentMessage.index] = sentMessage.time;
      }
      localStorage.setItem("latestEvents", JSON.stringify(this.latestEvents));
    },
  },
};
</script>
<style>
@import "stylesheet.scss";
</style>
