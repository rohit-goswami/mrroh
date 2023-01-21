---
title: "EVM Puzzles Writeups"
date: 2023-01-14T16:09:02+05:30
draft: false
cover:
    image: img/puzzles.png
    alt: 'walking down streets of EVM opcode secrets'
    caption: 'walking down streets of EVM opcode secrets'
tags: ["Ethereum", "EVM"]
categories: ["tech"]
description: "As part of this series of posts, I will share my approach to solving EVM puzzles as well as my learnings from the process."
---

Understanding EVM opcodes in gamified way!. To fully understand Ethereum you must understand **EVM (Ethereum Virtual Machine)**. I have recently been solving EVM puzzles, which have helped me gain a better understanding of EVM and opcodes. There are 3 puzzle series so far that I had found on the internet

Puzzles: 
* [By fvictorio](https://github.com/fvictorio/evm-puzzles)
* [By daltboy](https://github.com/daltyboy11/more-evm-puzzles)
* [By mattaereal](https://github.com/mattaereal/yet-another-evm-puzzle/)

WAGMI! Let's get started.


## Puzzle 1
```yaml
00      34      CALLVALUE
01      56      JUMP
02      FD      REVERT
03      FD      REVERT
04      FD      REVERT
05      FD      REVERT
06      FD      REVERT
07      FD      REVERT
08      5B      JUMPDEST
09      00      STOP
```
<details>
    <summary><span style="color:red">Reveal Solution</span></summary>
 
Our end goal is to provide right callvalue to make the correct execution of JUMP code and next instruction set should start from JUMPDEST. Note the offset for jumpdest is 8. therefore we can pass 8 as callvalue and woof! puzzle solved.



![](https://i.imgur.com/kG6OjxD.png)

</details>


### More puzzles writeups will share soon! :) 


