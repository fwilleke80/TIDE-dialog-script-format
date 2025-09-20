# TIDE  (Talk Interaction & Dialog Encoding)

_Version 1.0_  
_2025-09-20_  
_by Frank Willeke (<https://www.frankwilleke.de>)_

## “Ride the TIDE — simple scripts for complex talks.”

_TIDE_ is a lightweight, human-readable format for writing branching conversations.

It was developed for _The Sardine Identity_ but is flexible enough for any adventure game.

_TIDE_ scripts use simple blocks and commands to encode dialog, narration, conditions, and options - making it easy to draft, read, and expand dialog trees without engine-specific tools.

This document proposes and defines a format to write the game's dialogs in. That way, the dialogs can be written not unlike in a screenplay, but still offer a tree-like structure.

## General Rules

* Every dialog structure is enclosed in square brackets [ … ].
* Indentation shows hierarchy, but brackets always close the block clearly.
* Control markers (`>>`) define logic, flow, or special actions.

👉 This block-enclosed style gives you:

* __Structure__ (easy to scan, no dangling options),
* __Consistency__ (every branch ends cleanly),
* __Flexibility__ (conditions, randomness, nested branches),
* __Readability__ (still reads like noir dialog).

## Elements

### Root

```ruby
ROOT: Talk to Coraline
[ … ]
```

* Defines the entry point for this NPC’s conversation.

### Dialog Lines & Narration

```ruby
SAL: Line.
CORALINE: Line.
NARRATION (SAL): Internal thought or voiceover.
```

* Plain screenplay style.
* Narration is labeled explicitly.

### Control markers

Anything happens, occurs, or controls the dialog flow is written in all capital letters and started with `>>`, to make it immediately visible. There are a few recommended standard control markers, but basically it can be anything needed for the specific game project.

Examples for common control markers are as follows.

#### Conditions

```ruby
>> IF TALKING FOR THE FIRST TIME
[ … ]
>> ELSE
[ … ]
```

* Handles different flow depending on state.
* You can extend with `>> IF AFTER FLASHBACK`, etc..

#### Options

Defines dialog options the player can choose from.

```ruby
>> OPTIONS
[
   1. Option text
   [ … ]
   2. Another option
   [ … ]
]
```

* Each numbered option shows the choice text.
* Inside its block you script the branch.

To make things clearer, you can optionally name an `OPTIONS` block. This makes it more comprehensive where a `RETURN` control marker leads to.

```ruby
>> OPTIONS (what_to_do)
[
   1. Let´s investigate.
   [
      SAL: Let´s investigate.
      NPC: Yeah!

      >> RETURN TO what_to_do
   ]
   …
]
```

#### Randomization

Defines a random choice from a bunch of things that could happen.

```ruby
>> RANDOM
[
   1. [ … ]
   2. [ … ]
   3. [ … ]
]
```

* One sub-block is chosen at random.
* Good for varied bartender quips, drink specials, etc.

#### Flow Control

Exampels of flow control markers:

`>> RETURN`: Ends this branch and goes back to the current dialog level.  
`>> RETURN UP`: Ends this branch and jumps one dialog level higher (useful in nested options).  
`>> EXIT`: Ends the dialog.  
`>> JUMP`: Jumps to another dialog tree.  
`>> Flashback` (or similar): Scene change / special trigger.

#### Other markers

Basically, markers can be anything. Like stage directions.

`>> SAL scratches his butt.`  
`>> A three-headed monkey shows up.`  
`>> CORALINE smiles.`

### Comments

Comments can be introduced using `#`.

```ruby
[
   SAL: Say, Coraline...
   …
   # Maybe there's a better saying for this:
   CORALINE: Shiver me timbers!
]
```

### Editing

Any text editor will work!

For the most comfortable writing experience, use a text editor that offers proper indentation. For example:

* Atom (<https://atom-editor.cc>)
* Notepad++ (<https://notepad-plus-plus.org>)
* Sublime Text (<https://www.sublimetext.com>)
* Visual Studio Code (<https://code.visualstudio.com>)

#### Syntax highlighting

Of course, there is not official syntax highlighter or linter for _TIDE_, but I found that Ruby works quite well.

## Example

```ruby
ROOT: Talk to Coraline
[
   >> IF TALKING FOR THE FIRST TIME
   [
      SAL: Hey, Coraline.
      CORALINE: You look awful.
      SAL: Thanks. I feel awful, too.
      CORALINE: What happened to you?

      >> SAL rolls his eyes.

      SAL: Don´t ask.
   ]
   >> ELSE
   [
      SAL: Hey.
      CORALINE: Back for more, Sal?
   ]

   >> OPTIONS
   [
      1. What happened yesterday?
      [
         SAL: What happened yesterday? Can´t remember anything.
         CORALINE: You wouldn´t believe if I told you.

         >> CORALINE smiles sarcastically.
         
         >> OPTIONS
         [
            1. Try me.
            [
               SAL: Try me.
               CORALINE: Well, you came in pretty drunk already...
               …

               >> RETURN UP
            ]
            2. You´re right.
            [
               SAL: You´re right, I probably don´t wanna hear it.
               CORALINE: It´s for the best.
               …

               >> RETURN UP
            ]
         ]

         >> RETURN
      ]
      2. Bye.
      [
         SAL: I´ll take off, bye.

         >> RANDOM
         [
            1.
            [
               CORALINE: Take care, Sal.
            ]
            2.
            [
               CORALINE: Stay alert, Sal.
            ]
            3.
            [
               CORALINE: Be careful, Sal.
            ]
         ]

         >> EXIT
      ]
   ]
]
```
