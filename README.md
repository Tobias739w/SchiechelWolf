<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<title>Blackjack</title>
<link rel="stylesheet" href="style.css">
</head>

<body>

<h1>Blackjack</h1>

<div class="game">

<div class="cards">
<h2>Dealer</h2>
<p id="dealerCards"></p>
<p id="dealerSum"></p>
</div>

<div class="cards">
<h2>Spieler</h2>
<p id="playerCards"></p>
<p id="playerSum"></p>
</div>

<p id="result"></p>

<div class="buttons">
<button onclick="startGame()">Start</button>
<button onclick="hit()">Karte ziehen</button>
<button onclick="stand()">Stand</button>
</div>

</div>

<script src="script.js"></script>
</body>
</html>
body{
font-family: Arial;
background: #0b6623;
color: white;
text-align: center;
}

.game{
margin-top:50px;
}

.cards{
margin:20px;
}

button{
padding:10px 20px;
margin:10px;
font-size:16px;
cursor:pointer;
border:none;
border-radius:5px;
}

button:hover{
background:#ddd;
color:black;
}
let deck = []
let playerCards = []
let dealerCards = []

function createDeck(){
    deck = []
    let suits = ["♠","♥","♦","♣"]
    let values = ["A","2","3","4","5","6","7","8","9","10","J","Q","K"]

    for(let suit of suits){
        for(let value of values){
            deck.push(value + suit)
        }
    }
}

function getValue(card){
    let value = card.slice(0,-1)

    if(value === "A") return 11
    if(["K","Q","J"].includes(value)) return 10
    return parseInt(value)
}

function drawCard(){
    let random = Math.floor(Math.random()*deck.length)
    return deck.splice(random,1)[0]
}

function startGame(){

    createDeck()

    playerCards = [drawCard(), drawCard()]
    dealerCards = [drawCard()]

    updateUI()
}

function hit(){

    playerCards.push(drawCard())
    updateUI()

    if(getSum(playerCards) > 21){
        document.getElementById("result").textContent = "Du hast verloren!"
    }

}

function stand(){

    while(getSum(dealerCards) < 17){
        dealerCards.push(drawCard())
    }

    let playerSum = getSum(playerCards)
    let dealerSum = getSum(dealerCards)

    if(dealerSum > 21 || playerSum > dealerSum){
        result = "Du gewinnst!"
    }
    else if(playerSum === dealerSum){
        result = "Unentschieden!"
    }
    else{
        result = "Dealer gewinnt!"
    }

    document.getElementById("result").textContent = result
    updateUI()

}

function getSum(cards){

    let sum = 0
    for(let card of cards){
        sum += getValue(card)
    }
    return sum

}

function updateUI(){

    document.getElementById("playerCards").textContent = playerCards.join(" ")
    document.getElementById("dealerCards").textContent = dealerCards.join(" ")

    document.getElementById("playerSum").textContent = "Summe: " + getSum(playerCards)
    document.getElementById("dealerSum").textContent = "Summe: " + getSum(dealerCards)

}
blackjack-website
│
├── index.html
├── style.css
└── script.js
