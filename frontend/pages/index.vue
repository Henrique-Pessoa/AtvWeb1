<script setup>
let response = ref("");
const dialog = reactive({
  text: "",
  type: "",
  name: "",
  image: "",
  historyId: null,
});

const includeDialog = (type) => {
  if (type === "Q") {
    dialog.image = "user.png";
    dialog.name = "André";
    dialog.type = "right";
  } else {
    dialog.image = "bot.png";
    dialog.name = "Bot";
    dialog.type = "left";
  }
  conversationHistory.value.push(JSON.parse(JSON.stringify(dialog)));
};

const sendMessage = async () => {
  console.log(dialog.text);
  includeDialog("Q");

  const { data: answer } = await useFetch("http://localhost:8000/chatbot/", {
    method: "POST",
    body: {
      question: dialog.text,
      userId: 1,
      conversationId: dialog.historyId,
    },
  });
  dialog.text = answer.value.message;
  dialog.historyId = answer.value.history.id;
  includeDialog("A");
  dialog.text = "";
};

const conversationHistory = ref([]);
const showHistory = ref(false)

</script>

<template>
  <nav class="fixed w-screen h-14 bg-slate-700">
  <svg @click="()=>!showHistory" class="mt-3 cursor-pointer" xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 2048 2048"><path fill="white" d="M2048 128v128H0V128zM0 1664h2048v128H0zm0-768h2048v128H0zm0-384h2048v128H0zm0 768h2048v128H0z"></path></svg>
  </nav>
  <div v-if="showHistory" class="w-28 h-28 bg-red-600"></div>
  <div class="bg-slate-900 h-screen w-screen">
    <div
      v-for="(conversation, id) in conversationHistory"
      :key="id"
      class="mb-4"
    >
      <TextBox
        :name="conversation.name"
        :avatarImage="conversation.image"
        :message="conversation.text"
        :type="conversation.type"
      />
    </div>
    <div class="absolute mt-14">
      <label for="" class="text-white"> Type here your message!</label> <br />
      <textarea class="text-black" v-model="dialog.text" /> <br />
      <br />
      <button class="bg-green-600 w-20 rounded-xl" @click="sendMessage">
        Send
      </button>
      <hr />
      <div class="w-full h-32">
        <h5 class="text-white">Bard: 😎</h5>
        <p class="text-red-500 text-6xl">{{ response }}</p>
      </div>
    </div>
  </div>
</template>
