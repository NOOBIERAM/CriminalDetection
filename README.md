
# Member of a group
- RASOLOMANDIMBY Tendry Antenaina Steeve 2585
- RAMAMNATSOA Rotsiniaina 2691
- RICHARTS Diary Fenohasina Ravalo 2741
- RAKOTONDRAZANDRY Kenny Urvano 2761

---

# Criminal Detection System

An interactive **Criminal Detection System** built with **Vue.js 3** and **Bootstrap 5**.  
It allows users to select a suspect and a crime type to determine guilt based on predefined rules and evidence.

---
## Table of Contents

- [Features](#features)   
- [Installation](#Installation)  
- [Usage](#usage)  
- [Example of Evidence](#example-of-evidence)  
- [Technologies](#technologies)  

---

## Features

- Select a suspect from a list: `john`, `mary`, `alice`, `bruno`, `sophie`  
- Select a crime type: `vol`, `assassinat`, `escroquerie`  
- **Always visible notes** for culpability rules:  
  - **Vol** → Mobile + Presence + Fingerprint  
  - **Assassinat** → Mobile + Presence + (Fingerprint or Eyewitness)  
  - **Escroquerie** → Mobile + (Bank Transaction or Fake Identity)  
- **“Check Guilt” button** to display result (`Guilty` / `Not Guilty`) and evidence  
- Detailed display of evidence/indices for each suspect and crime  

---


## Installation

### Prerequisites

- [Node.js](https://nodejs.org/) >= 16
- [npm](https://www.npmjs.com/) >= 8

### Steps

1. **Clone the repository** :
```bash
git clone git@github.com:NOOBIERAM/CriminalDetection.git
```
2. **Navigate into the project folder** :
```bash
cd CriminalDetection
```
3. **Install dependencies** :
```bash
npm install
```
4. **Run the project** :
```bash
npm run serve
```
5. **Open the app in your browser (default url)** :
```bash
http://localhost:5173
```

