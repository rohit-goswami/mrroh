---
title: "EVM Puzzles Writeups"
date: 2023-01-14T16:09:02+05:30
draft: false
cover:
    image: img/puzzles.png
    alt: 'walking down streets of EVM opcode secrets'
    caption: 'walking down streets of EVM opcodes secrets'
tags: ["Ethereum", "EVM"]
categories: ["tech"]
description: "As part of this series of posts, I will share my approach to solving EVM puzzles as well as my learnings from the process."
---

Understanding EVM opcodes in gamified way!. To fully understand Ethereum you must understand **EVM (Ethereum Virtual Machine)**. I have recently been solving EVM puzzles, which have helped me gain a better understanding of EVM and opcodes. There are 3 puzzle series so far that I had found on the internet

Puzzles: 
* [By fvictorio](https://github.com/fvictorio/evm-puzzles)
* [By daltboy](https://github.com/daltyboy11/more-evm-puzzles)
* [By mattaereal](https://github.com/mattaereal/yet-another-evm-puzzle/)

WAGMI! Let's get started. For better understanding of these puzzles, I would recommend you to read EVM deep dives series by noxx, that series will help you to understand EVM in depth.


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
 
This weird looking series of codes are actually EVM opcodes, which represents a smart contract. Make use of [EVM Playground](https://www.evm.codes/playground?fork=grayGlacier&unit=Wei&codeType=Mnemonic&code='CALLVALUEy~~~~~~yDESTzSTOP'~zREVERTz%5CnyzJUMP%01yz~_) to get more comfortable with the opcodes. This contract asking a value to be send in a transaction so that it won't hit REVERT opcode. First you need to understand about [CALLVALUE](https://www.evm.codes/#34?fork=grayGlacier) opcode. In short this opcode gets value of the current call in wei and pushes that to the top of stack. Next we have JUMP opcode which simply takes the top value on the stack and jumps to the 



![](https://i.imgur.com/kG6OjxD.png)

</details>


### More puzzles writeups will share soon! :) 