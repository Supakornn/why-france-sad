<script>
import TailwindCss from './TailwindCSS.svelte';
import { initializeApp } from "firebase/app";
import {getFirestore, collection, onSnapshot, getDocs} from "firebase/firestore"

  const firebaseConfig = {
    apiKey: "AIzaSyDcYc0bx1SJEtK7nPrrfFyDFGuQeg0IMRQ",
    authDomain: "whyfrancesad.firebaseapp.com",
    projectId: "whyfrancesad",
    storageBucket: "whyfrancesad.appspot.com",
    messagingSenderId: "989914881222",
    appId: "1:989914881222:web:a0154ce5abbd5133fc4bc7"
  };
  const app = initializeApp(firebaseConfig);
  const db = getFirestore();
  const colRef = collection(db, "allanswer");
  let allanswers = [];

   const unsub = onSnapshot(colRef, (querySnapshot) => {
    let answers = [];
    querySnapshot.forEach((doc) => {
      let answer = {id: doc.id, ...doc.data()};
      answers = [...answers, answer];
    })
    allanswers = answers;
    console.log(allanswers);
  });

  function prev() {
   
  }
  function next() {
    
  }
  
</script>
<TailwindCss />
<link href="https://fonts.googleapis.com/css2?family=Sarabun&display=swap" rel="stylesheet" />
<main class="h-screen grid place-items-center">

<div class="canvas select-none flex flex-col justify-center items-center rounded-xl p-4 md:p-8 my-8 native-font w-full md:mx-auto md:w-4/5 lg:w-2/3 2xl:w-1/2">
  
  <h1 class="title m-4 text-4xl text-black font-bold text-center">>>>>>>>>>></h1>
  <div class="desc w-full bg-white my-5 mb-8 rounded-md">
    <p class="description m-4 text-2xl text-black text-center ">{allanswers[0]}</p>
  </div>
 <div class="btnCon flex justify-between w-full  ">
  <button on:click={() => prev()} class="btn shadow-lg left-0 px-4 py-2  bg-red-600 rounded-lg  text-white transition duration-200 ease-in-out">ย้อนกลับ</button>
  <button on:click={() => next()} class="btn shadow-lg right-0 px-4 py-2  bg-green-600 rounded-lg  text-white transition duration-200 ease-in-out">ถัดไป</button>
 </div>
 
  
</div>


</main>

<style lang="postcss">
  .canvas {
    background-color: #b9a0ee;
  }
  
  :root {
    font-family: "Sarabun", sans-serif;
  }

</style>