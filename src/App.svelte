<script>
  import TailwindCss from "./TailwindCSS.svelte";
  import firebase from "firebase/compat/app";
  import {
    collection,
    onSnapshot,
    addDoc,
  } from "firebase/firestore";
  import "firebase/compat/firestore";

  const firebaseConfig = {
    apiKey: "AIzaSyDcYc0bx1SJEtK7nPrrfFyDFGuQeg0IMRQ",
    authDomain: "whyfrancesad.firebaseapp.com",
    projectId: "whyfrancesad",
    storageBucket: "whyfrancesad.appspot.com",
    messagingSenderId: "989914881222",
    appId: "1:989914881222:web:a0154ce5abbd5133fc4bc7"
  };
  const firebaseApp = firebase.initializeApp(firebaseConfig);
  const db = firebaseApp.firestore();
  const colRef = collection(db, "allanswer");
  let allanswers = [];
  let all = [];

  const unsub = onSnapshot(colRef, (querySnapshot) => {
    let answers = [];
    querySnapshot.forEach((doc) => {
      let answer = { id: doc.id, ...doc.data() };
      answers = [...answers, answer];
    });
    allanswers = answers;
    allanswers.sort((a, b) => a.id - b.id);
    console.log(allanswers);
    all = allanswers.map((answer) => {
      return answer.answer;
    });
    console.log(all);
  });

  let i = 0;
  const prev = () => {
    if (i <= 0) {
      i = 0;
    } else {
      i -= 1;
    }
  };

  const next = () => {
    if (i >= allanswers.length - 1) {
      i = allanswers.length - 1;
    } else {
      i += 1;
    }
    console.log(i);
    console.log(allanswers);
  };

  const showform = () => {
    let form = document.getElementById("form");
    form.style.display = "block";
  };

  let msg = "";
  const submit = () => {
    addDoc(collection(db, "allanswer"), {
      answer: msg,
      id: allanswers.length + 1
    }).then(() => {
      msg = "";
      let form = document.getElementById("form");
      form.style.display = "none";
    });
  };
</script>

<TailwindCss />
<link href="https://fonts.googleapis.com/css2?family=Sarabun&display=swap" rel="stylesheet" />
<main class="h-screen grid place-items-center">
  <div class="w-full ">
    <h1 class="text-white title m-4 text-6xl font-bold text-center mb-20">ทำไมฟร้องถึงอกหัก 🤔</h1>
    <div
      class="canvas select-none flex flex-col justify-center items-center rounded-xl p-4 md:p-8 my-8 native-font w-full md:mx-auto md:w-4/5 lg:w-2/3 2xl:w-1/2"
    >
      <h1 class="title m-4 text-4xl text-black font-bold text-center">เพราะอะไรกันนะ?</h1>
      <div class="desc w-full bg-white my-5 mb-8 rounded-md">
        <p class="description m-4 text-2xl text-black text-center ">{all[i]}</p>
      </div>
      <div class="btnCon flex justify-between w-full  ">
        <button
          on:click={() => prev()}
          class="btn shadow-lg left-0 px-4 py-2  bg-red-600 rounded-lg  text-white transition duration-200 ease-in-out"
          >ย้อนกลับ</button
        >
        <button
          on:click={() => next()}
          class="btn shadow-lg right-0 px-4 py-2  bg-green-600 rounded-lg  text-white transition duration-200 ease-in-out"
          >ถัดไป</button
        >
      </div>
      <div class="flex flex-col my-5 mt-10">
        <button
          on:click={() => {
            showform();
          }}
          class="btn shadow-lg left-0 px-4 py-2  rounded-xl bg-blue-600 text-white transition duration-200 ease-in-out mb-2"
          >เพิ่มคำตอบ</button
        >
        <div id="form" class="hideform">
          <form on:submit|preventDefault={submit}>
            <input
              type="text"
              class="w-80 h-12 rounded-xl px-3 outline-none"
              placeholder="เพิ่มคำตอบแล้วกด enter"
              bind:value={msg}
            />
          </form>
        </div>
      </div>
    </div>
    <h1 class="text-white title m-4 text-sm font-bold text-center mb-20">Made with ❤ by แก๊ปเพื่อนรัก</h1>
  </div>
</main>

<style lang="postcss">
  main {
    background: #000;
  }

  /* .hideform {
    display: none;
  } */

  #form {
    display: none;
  }

  .canvas {
    background-color: #b9a0ee;
  }

  :root {
    font-family: "Sarabun", sans-serif;
  }
</style>
