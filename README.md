<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Guess my number</title>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap"
      rel="stylesheet"
    />
   <!-- <link rel="stylesheet" href="style-guessTheNumber.css" />-->
  </head>

<style>
  * {
  margin: 0;
  padding: 0;
}

html {
  font-size: 62.5%;
  font-family: "Josefin Sans", sans-serif;

  @media (max-width: 500px) {
    font-size: 50%;
  }
}

body {
  background-color: #333;
}

.head-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  align-items: center;
  justify-content: center;
  margin-top: 3rem;

  @media (max-width: 500px) {
    display: flex;
    flex-direction: column;
  }
}

.heading-1 {
  color: white;
  grid-column: 2 / 3;
  font-size: 4.5rem;
  text-align: center;
}

.leaderboard-btn {
  width: 20rem;
  margin: 0 auto;
  font-family: inherit;
  height: 4rem;
  border-radius: 5px;
  background-color: white;
  border: none;
  font-size: 1.8rem;
  cursor: pointer;
  transition: all 0.5s;

  @media (max-width: 500px) {
    margin-top: 2rem;
  }
}

.leaderboard-btn:hover {
  border: 2px solid white;
  background-color: black;
  color: white;
}

.leaderboard {
  display: block;
  margin: 5rem auto;
  width: 50rem;
  color: white;
  font-size: 2rem;
  background-color: black;
  padding: 2rem 2.5rem;
  border-radius: 11px;
  transition: all 0.5s;

  @media (max-width: 500px) {
    width: 30rem;
  }
}

.hidden {
  display: none;
}

.text {
  color: white;
  text-align: center;
  font-size: 2.3rem;
  margin-top: 3rem;
}

.hidden-number {
  display: block;
  width: 13rem;
  height: 13rem;
  margin: 2.5rem auto;
  background-color: white;
  display: flex;
  align-items: center;
  justify-content: center;
}

.hidden-number p {
  font-size: 3.5rem;
}

.guesses {
  font-size: 1.7rem;
  text-align: center;
  margin-top: 2rem;
  color: white;
}

.input {
  display: block;
  margin: 1rem auto;
  height: 3rem;
  text-align: center;
  font-size: 2rem;
  width: 10rem;
  font-family: inherit;
}

.check {
  display: block;
  margin: 1rem auto;
  height: 3rem;
  width: 8rem;
  font-family: inherit;
  font-size: 1.7rem;
  padding: 0.5rem 1.5rem;
  cursor: pointer;
  border-radius: 5px;
  border: none;
  background-color: gold;
}

.try-again {
  display: block;
  margin: 1rem auto;
  width: 10rem;
  height: 3rem;
  font-family: inherit;
  font-size: 1.7rem;
  border: none;
  border-radius: 5px;
  background-color: white;
  transition: all 0.5s;
  visibility: hidden;
  opacity: 0;
  cursor: pointer;
}

.try-again:hover {
  color: white;
  background-color: black;
  border: 2px solid white;
}

.main {
  position: relative;
}

.ranked {
  position: absolute;
  top: 0%;
  left: 10%;

  @media (max-width: 500px) {
    bottom: 0%;
    top: 110%;
    left: 20%;
  }
}

.ranked-img {
  height: 25rem;
  width: 28rem;
}

.text-rank {
  font-size: 3rem;
  color: white;
  margin-bottom: 2rem;
}

</style>
  
  <body>
    <div class="head-grid">
      <h1 class="heading-1">Guess my number</h1>
      <button class="leaderboard-btn">Leaderboard🏆</button>
    </div>

    <div class="leaderboard hidden">
      Leaderboard is currently not in function:( Please try again later.
    </div>

    <section class="main">
      <p class="text">Start guessing...</p>
      <input class="input" type="number" />
      <button class="check">Check!!!</button>
      <button class="try-again">Try Again</button>
      <figure class="hidden-number"><p class="number">?</p></figure>
      <p class="guesses">
        Number of guesses: <span class="guess-counter">5</span>
      </p>
      <div class="ranked">
        <p class="text-rank">
          You are in <span class="rank">bronze</span> rank
        </p>
        <img src="bronzeRank.png" class="ranked-img" />
      </div>
    </section>

    <script>
      const leaderboardBtn = document.querySelector(".leaderboard-btn");
const leaderboard = document.querySelector(".leaderboard");
const input = document.querySelector(".input");
const btnCheck = document.querySelector(".check");
const body = document.querySelector("body");
const message = document.querySelector(".text");
const displayNumber = document.querySelector(".number");
const guessMessage = document.querySelector(".guess-counter");
const btnTryAgain = document.querySelector(".try-again");
const rank = document.querySelector(".rank");
const rankImg = document.querySelector(".ranked-img");

const ranks = [
  "bronze",
  "silver",
  "gold",
  "diamond",
  "mythic",
  "legendary",
  "masters",
  "pro",
];
const rankImages = {
  bronze: "bronzeRank.png",
  silver: "silverRank.png",
  gold: "goldRank.png",
  diamond: "diamondRank.png",
  mythic: "mythicRank.png",
  legendary: "legendaryRank.png",
  masters: "mastersRank.png",
  pro: "proRank_1-removebg-preview.png",
};
const leaderboardKey = "leaderboardData";

let guess = 5;
let i = 0;
let secretNumber = Math.trunc(Math.random() * 50) + 1;
console.log(secretNumber);

function getLeaderboard() {
  return JSON.parse(localStorage.getItem(leaderboardKey)) || [];
}

function updateLeaderboard(playerName, playerRank) {
  let leaderboard = getLeaderboard();

  // Ako igrač već postoji, ne tražimo unos imena
  let existingPlayer = leaderboard.find((player) => player.name === playerName);
  if (
    existingPlayer &&
    ranks.indexOf(playerRank) <= ranks.indexOf(existingPlayer.rank)
  ) {
    return;
  }

  // Dodajemo novog igrača ako prestigne nekoga
  leaderboard.push({ name: playerName, rank: playerRank });
  leaderboard.sort((a, b) => ranks.indexOf(b.rank) - ranks.indexOf(a.rank));
  leaderboard = leaderboard.slice(0, 5);
  localStorage.setItem(leaderboardKey, JSON.stringify(leaderboard));
  displayLeaderboard();
}

function displayLeaderboard() {
  const leaderboardData = getLeaderboard();
  leaderboard.innerHTML =
    leaderboardData.length === 0
      ? "No players yet."
      : "<h2>Leaderboard 🏆</h2><ol>" +
        leaderboardData
          .map((player) => `<li>${player.name} - ${player.rank}</li>`)
          .join("") +
        "</ol>";
}

document.addEventListener("DOMContentLoaded", displayLeaderboard);

const againFunction = function () {
  if (i === 7) {
    message.textContent = "You won🏆";
    input.style.display = "none";
    btnCheck.style.display = "none";
    btnTryAgain.style.opacity = "1";
    btnTryAgain.style.visibility = "visible";
  }

  secretNumber = Math.trunc(Math.random() * 50) + 1;
  guess = 5;
  input.value = "";
  guessMessage.textContent = guess;
  message.textContent = "Start guessing...";
  displayNumber.textContent = "?";
  rank.textContent = ranks[i];
  rankImg.src = rankImages[ranks[i]];
  console.log(secretNumber);
};

btnCheck.addEventListener("click", function () {
  let inputNumber = +input.value;

  if (inputNumber === secretNumber) {
    displayNumber.textContent = secretNumber;
    message.textContent = "Correct✅";

    setTimeout(() => {
      let leaderboard = getLeaderboard();
      let lowestRankInTop5 =
        leaderboard.length < 5
          ? null
          : leaderboard[leaderboard.length - 1].rank;

      // Proveravamo da li igrač treba da unese ime
      if (
        leaderboard.length < 5 ||
        ranks.indexOf(ranks[i]) > ranks.indexOf(lowestRankInTop5)
      ) {
        let playerName = prompt("Čestitamo! Unesi svoje ime za leaderboard:");
        if (playerName) updateLeaderboard(playerName, ranks[i]);
      }

      againFunction();
    }, 1500);

    i++;
  } else {
    message.textContent =
      inputNumber < secretNumber ? "Too low📉" : "Too high📈";
    guess--;
    guessMessage.textContent = guess;
  }

  if (guess === 0) {
    message.textContent = "You lost try again❌";
    input.style.display = "none";
    btnCheck.style.display = "none";
    btnTryAgain.style.opacity = "1";
    btnTryAgain.style.visibility = "visible";
  }
});

btnTryAgain.addEventListener("click", function () {
  btnTryAgain.style.opacity = "0";
  btnTryAgain.style.visibility = "hidden";
  guess = 5;
  input.style.display = "block";
  btnCheck.style.display = "block";
  secretNumber = Math.trunc(Math.random() * 50) + 1;
  guessMessage.textContent = guess;
  message.textContent = "Start guessing...";
  input.value = "";
  rank.textContent = "bronze";
  rankImg.src = "bronzeRank.png";
  i = 0;
});

leaderboardBtn.addEventListener("click", function () {
  leaderboard.classList.toggle("hidden");
});

    </script>
  </body>
</html>
