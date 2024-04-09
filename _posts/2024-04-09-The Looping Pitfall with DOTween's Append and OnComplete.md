---
layout: single
title:  "The Looping Pitfall with DOTween's Append and OnComplete"
categories: "Unity"
Tag: [Unity, Animation, DOTween]
toc: true
---

## The Looping Pitfall with DOTween's Append and OnComplete
Today, I want to chat about a sneaky issue I stumbled upon while tinkering with DOTween in Unity. It's all about the unexpected journey of a tween that got lost in a loop – literally. So, grab your coffee, and let's dive into the world of animations gone wild.

The Setup: A Common Scenario
Imagine you're working on a cool game feature where objects dance around with smooth animations. You decide to use DOTween because, let's face it, it's awesome for creating sleek, code-driven animations in Unity. You whip up a sequence, append some animations, and, for that cherry on top, add a loop to keep the party going. Something like this:


```
var mySequence = DOTween.Sequence();
mySequence.Append(someTransform.DOMoveX(5, 1));
mySequence.Append(someTransform.DORotate(new Vector3(0, 180, 0), 1));
mySequence.SetLoops(-1, LoopType.Restart);
```
The Twist: An Unexpected Behavior
But then, you notice something odd. The object's supposed to stop dancing at some point – maybe when the player interacts with it or a certain condition is met. Naturally, you think, "No problemo, I'll just kill the sequence," and you add a kill command when the condition is met. it's easy, right? Wrong.

The animation keeps looping, completely ignoring your desperate pleas for it to stop. You double-check your code, and everything seems fine. What gives?

### The Culprit: Looping with Append and OnComplete
Here's the kicker: When you use Append in DOTween along with looping sequences, the OnComplete callbacks or any attempt to kill the sequence might not work as expected if the loop is infinite. Why? Because the sequence considers itself "never ending," it doesn't reach a point where it thinks, "Okay, time to wrap up and call it a day."

In our case, we had a sequence with an appended loop, and when we tried to introduce a new sequence or kill the current one upon a certain event, it was as if we were speaking a different language. The sequence was lost in its own loop-land, happily ignoring our commands.

The Fix: How to Get Your Sequence Back on Track
So, what's the solution? First, if you need a looping animation, consider using a finite number of loops that you can control and then explicitly stop or kill the sequence when necessary. Alternatively, you can avoid using loops within sequences that might need to be stopped or modified based on game events.

Here's a quick fix example:
```
// Create a looping animation without using SetLoops
var mySequence = DOTween.Sequence();
mySequence.Append(someTransform.DOMoveX(5, 1).SetLoops(3));
mySequence.Append(someTransform.DORotate(new Vector3(0, 180, 0), 1));
// Add a completion callback to handle what happens next
mySequence.OnComplete(() => Debug.Log("Done dancing!"));
```
### Wrapping Up
Animations in games are like spices in cooking: they add flavor and personality. DOTween is a fantastic tool in the Unity developer's arsenal, but like any powerful tool, it requires a bit of finesse to handle properly, especially when it comes to looping animations.

Remember, understanding how these animations interact with your game's logic is key to creating a seamless and engaging player experience. So next time you're setting up those cool moves, keep an eye out for the looping pitfall. Happy coding!

Written by [Matias Kong , NewWaveDog]