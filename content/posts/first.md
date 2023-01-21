---
title: "First"
date: 2023-01-14T16:09:02+05:30
draft: false
cover:
    image: img/puzzles.png
    alt: 'walking down streets of EVM opcode secrets'
    caption: 'walking down streets of EVM opcode secrets'
tags: ["Ethereum", "EVM"]
categories: ["tech"]
---

# EVM Puzzles Series

Understanding EVM opcodes in gamified way!

In this series of blog posts, I am going to share my approach and learning while solving EVM puzzles.

Puzzles: 
* [By fvictorio](https://github.com/fvictorio/evm-puzzles)
* [By daltboy](https://github.com/daltyboy11/more-evm-puzzles)
* [By mattaereal](https://github.com/mattaereal/yet-another-evm-puzzle/)

WAGMI! Let's get started.


## Puzzle 1
```json
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
 
Our end goal is to provide right callvalue which is in wei to make the JUMP execution jump to JUMPDEST. notice the offset for jumpdest is 8. therefore we can pass 8 as callvalue and yay! you solve it. 

:sunglasses: :checkered_flag:

![](https://i.imgur.com/kG6OjxD.png)

</details>


### More puzzles writeups will share soon! :) 


