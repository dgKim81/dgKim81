
# 안녕하세요~! 동동 입니다.

안녕하세요~! 동동 입니다. 저의 Git hub 저장소에 오신 것을 환영합니다.

💡 **Full-Stack Developer** | 🌱 Learning **DevSecOps, MLOps**

---

### ⚡ **Tech Stack**
<p align="left">
  <img src="https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=java&logoColor=white">
  <img src="https://img.shields.io/badge/C%23-239120?style=flat-square&logo=c-sharp&logoColor=white">
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white">
  <img src="https://img.shields.io/badge/SQL-CC2927?style=flat-square&logo=microsoft-sql-server&logoColor=white">
  <img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black">
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=node.js&logoColor=white">
  <img src="https://img.shields.io/badge/Vue.js-4FC08D?style=flat-square&logo=vue-dot-js&logoColor=white">
  <img src="https://img.shields.io/badge/.NET%20Framework-512BD4?style=flat-square&logo=dotnet&logoColor=white">
  <img src="https://img.shields.io/badge/.NET%20Core-512BD4?style=flat-square&logo=dotnet&logoColor=white">
  <img src="https://img.shields.io/badge/Spring-6DB33F?style=flat-square&logo=spring&logoColor=white">
  <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=spring-boot&logoColor=white">
  <img src="https://img.shields.io/badge/MS%20SQL-CC2927?style=flat-square&logo=microsoft-sql-server&logoColor=white">
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white">
  <img src="https://img.shields.io/badge/Oracle-F80000?style=flat-square&logo=oracle&logoColor=white">
  <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white">
</p>

---

blog: [dgkim81.github.io](https://dgkim81.github.io)  
E-main: [bravecat@nate.com](mailto:bravecat@nate.com)

<script>
const grid = document.querySelector(".js-calendar-graph-table");

const trs = document.querySelectorAll(".js-calendar-graph-table tr");
const colors = [
    "rgb(173, 216, 230)",  // Light Blue
    "rgb(150, 200, 220)",  // Soft Blue
    "rgb(120, 170, 190)",  // Grayish Blue
    "rgb(90, 140, 160)",   // Muted Blue-Gray
    "rgb(60, 110, 130)"    // Dark Blue-Gray
];
const directions = "TBRL";
const p2d = [];

let preMove = "";
const tail_length = 5;
let currentItems = [];
let currentPosition_x = 0;
let currentPosition_y = 0;

trs.forEach(tr => {
    tds = tr.getElementsByTagName("td")
    c_p2d = [];
    for (let i = 0; i < tds.length;i++) {
        if (tds.item(i).className == "ContributionCalendar-day") {
            c_p2d.push(tds.item(i))
            currentPosition_x = i;
            currentPosition_y = p2d.length - 1;
        } else {
            c_p2d.push(undefined)
        }
    }
    p2d.push(c_p2d);
});

function validDirections(x , y) {
    let validDir = directions.replace(preMove, "");

    if (y + 1 >= p2d.length || x >= p2d[y + 1].length || !p2d[y + 1][x]) {
        validDir = validDir.replace("B", "");
    } else if (currentItems.findIndex(fn => fn.tag == p2d[y + 1][x]) !== -1) {
        validDir = validDir.replace("B", "");
    }

    if (y <= 0 || !p2d[y - 1][x]) {
        validDir = validDir.replace("T", "");
    } else if (currentItems.findIndex(fn => fn.tag == p2d[y - 1][x]) !== -1) {
        validDir = validDir.replace("T", "");
    }

    if (x >= p2d[y].length || !p2d[y][x + 1]) {
        validDir = validDir.replace("R", "");
    } else if (currentItems.findIndex(fn => fn.tag == p2d[y][x + 1]) !== -1) {
        validDir = validDir.replace("R", "");
    }

    if (x <= 0 || !p2d[y][x - 1]) {
        validDir = validDir.replace("L", "");
    } else if (currentItems.findIndex(fn => fn.tag == p2d[y][x - 1]) !== -1) {
        validDir = validDir.replace("L", "");
    }

    return validDir;
}

function decisionDirect(validDir, x, y) {
    const idx = Math.floor(Math.random() * validDir.length);
    const dir = validDir[idx];

    switch (dir) {
        case "B": 
            y = y + 1;
            break;
        case "T":
            y = y - 1;
            break;
        case "L":
            x = x - 1;
            break;
        case "R":
            x = x + 1;
            break;
        default:
            break;
    }

    return { x, y }
}

function updateGrid(x, y) {
    const currentItem = p2d[currentPosition_y][currentPosition_x];
    currentItems.push({
        tag: currentItem,
        backgroundColor: currentItem.style.backgroundColor
    });

    if (currentItems.length >= colors.length) {
        removeItem = currentItems.shift();
        removeItem.tag.style.backgroundColor = removeItem.backgroundColor;
    }

    for(let i = 0; i < currentItems.length; i++) {
        currentItems[i].tag.style.backgroundColor = colors[i];

    }

    setTimeout(move, 300);
}

function move() {
    let validDir = validDirections(currentPosition_x, currentPosition_y);
    let position = decisionDirect(validDir, currentPosition_x, currentPosition_y);
    currentPosition_x = position.x;
    currentPosition_y = position.y;
    updateGrid(currentPosition_x, currentPosition_y);
}

setTimeout(() => {
    updateGrid(5,5);
}, 3000);

  
</script>
