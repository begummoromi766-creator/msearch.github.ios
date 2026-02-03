# msearch.github.ios<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>M Search</title>

<style>
body{
    margin:0;
    padding:16px;
    font-family: Arial, sans-serif;
    background:#ffffff;
}
h1{
    text-align:center;
    color:red;
    font-size:32px;
}
#container{
    max-width:480px;
    margin:auto;
}
input{
    width:100%;
    padding:14px;
    font-size:18px;
    border:2px solid #ddd;
    border-radius:8px;
}
button{
    width:100%;
    margin-top:10px;
    padding:14px;
    font-size:18px;
    border:none;
    border-radius:8px;
    cursor:pointer;
}
#searchBtn{
    background:green;
    color:white;
}
#historyBtn{
    background:#f1f1f1;
}
#answer{
    margin-top:20px;
    font-size:20px;
}
#link{
    display:none;
    margin-top:10px;
    color:blue;
}
#historyBox{
    display:none;
    margin-top:15px;
    border-top:1px solid #ddd;
}
.history-item{
    padding:10px;
    border-bottom:1px solid #eee;
    cursor:pointer;
}
.history-item:hover{
    background:#f5f5f5;
}
#clearBtn{
    background:red;
    color:white;
    margin-top:10px;
}
</style>
</head>

<body>

<h1>M Search</h1>

<div id="container">

<input id="q" placeholder="Search here">

<button id="searchBtn" onclick="search()">Search</button>
<button id="historyBtn" onclick="toggleHistory()">History</button>

<div id="answer"></div>
<a id="link" target="_blank">🔍 Search this on Google</a>

<div id="historyBox">
    <h3>Search History</h3>
    <div id="historyList"></div>
    <button id="clearBtn" onclick="clearHistory()">Clear History</button>
</div>

</div>

<script>
const data = {
  "what is google":"Google is a search engine.",
  "what is python":"Python is a programming language.",
  "what is computer":"Computer is an electronic machine.",
  "what is internet":"Internet is a global network."
};

let history = JSON.parse(localStorage.getItem("msearch_history")) || [];

function saveHistory(q){
  if(!history.includes(q)){
    history.unshift(q);
    localStorage.setItem("msearch_history", JSON.stringify(history));
  }
}

function search(){
  const q = document.getElementById("q").value.toLowerCase().trim();
  const ans = document.getElementById("answer");
  const link = document.getElementById("link");

  link.style.display="none";
  if(!q){ ans.innerText=""; return; }

  saveHistory(q);
  renderHistory();

  if(data[q]){
    ans.innerText = data[q];
    ans.style.color = ["red","blue","green"][Math.floor(Math.random()*3)];
  }else{
    ans.innerText = "Answer not found";
    ans.style.color = "black";
    link.style.display="block";
    link.href="https://www.google.com/search?q="+encodeURIComponent(q);
  }
}

function toggleHistory(){
  const box = document.getElementById("historyBox");
  box.style.display = box.style.display==="none" ? "block" : "none";
  renderHistory();
}

function renderHistory(){
  const list = document.getElementById("historyList");
  list.innerHTML="";
  history.forEach(q=>{
    const div = document.createElement("div");
    div.className="history-item";
    div.innerText=q;
    div.onclick=()=>{
      document.getElementById("q").value=q;
      search();
    };
    list.appendChild(div);
  });
}

function clearHistory(){
  history=[];
  localStorage.removeItem("msearch_history");
  renderHistory();
}
</script>

</body>
</html>
