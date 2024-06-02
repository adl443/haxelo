# HaxElo.js

A simple library to calculate Elo rating changes for teams and individual players. Initially made for Haxball, but can be used in any sort of Elo rating system.

## Installation

To install HaxElo.js, you can use npm:

```bash
npm install haxelo
```

## Usage

Node.js
To use the library in a Node.js project, require it as follows:

```js
const HaxElo = require('haxelo');

// Team Elo Change Example
const red = [1200, 1300, 1400]; // Elo ratings for the red team
const blue = [1100, 1000, 900]; // Elo ratings for the blue team
const score = 1; // Red wins
const factor = 32; // K factor

const result = HaxElo.teamEloChange(red, blue, score, factor);
console.log(result); // Output: { redChange: 13, blueChange: -13 }

// Player Elo Change Example
const red = 1500;
const blue = 1400;
const score = 1; // red wins
const factor = 32; // K factor

const result = HaxElo.playerEloChange(playerA, playerB, score, factor);
console.log(result); // Output: { redChange: 10, blueChange: -10 }
```

If you want to add it in the web you can just copy the following code:

```js
const HaxElo = (() => {
    function teamEloChange(red, blue, score, factor) {
        if (red.length !== blue.length) throw new Error("Both teams must have the same size.");
        var w = red.reduce((a, x) => x + a, 0) / red.length;
        var l = blue.reduce((a, x) => x + a, 0) / blue.length;
        var E_w = 1 / (1 + Math.pow(10, (l - w) / 400));
        var E_l = 1 / (1 + Math.pow(10, (w - l) / 400));
        var S_w, S_l;
        if (score == 1) {
            S_w = 1;
            S_l = 0;
        }
        else if (score == 2) {
            S_w = 0;
            S_l = 1;
        }
        else if (score == 0) {
            S_w = 0.5;
            S_l = 0.5;
        }
        else {
            throw new Error("Score must be 0, 1 or 2.");
        }
        var result = { redChange: Math.round(factor * (S_w - E_w)), blueChange: Math.round(factor * (S_l - E_l)) };
        return result;
    }

    function playerEloChange(red, blue, score, factor) {
        var E_a = 1 / (1 + Math.pow(10, (blue - red) / 400));
        var E_b = 1 / (1 + Math.pow(10, (red - blue) / 400));
        var S_a, S_b;
        if (score === 1) {
            S_a = 1;
            S_b = 0;
        }
        else if (score === 2) {
            S_a = 0;
            S_b = 1;
        }
        else {
            S_a = 0.5;
            S_b = 0.5;
        }
        var result = { redChange: Math.round(factor * (S_a - E_a)), blueChange: Math.round(factor * (S_b - E_b)) };
        return result;
    }

    return {
        teamEloChange: teamEloChange,
        playerEloChange: playerEloChange
    };
})();
```

## Contributions

Contributions to haxelo are welcome! Feel free to fork this repository, make changes, and submit pull requests to enhance its functionality or improve its documentation.