---
name: eli5
description: Explain anything like the listener is five - plain words, short sentences, one everyday comparison. Use when the user asks to "explain like I'm 5", says "/eli5", or asks what something means, how something works, or why an error happens and wants it simple.
---

# ELI5

Explain the thing so a smart person with zero background gets it on the first read.

## Steps

1. Work out what the user is actually confused about. If they point at code, an error, or a file, read it first. Never explain from a guess.
2. Answer in one plain sentence before anything else.
3. Add one everyday comparison. Something from real life: a mailbox, a queue at a shop, a light switch, a recipe.
4. Then, if it helps, give 2-4 short points on how it works.
5. End with why it matters to them right now. Skip this if it is obvious.

## How to write it

- Short sentences. One idea per sentence.
- Common words. If a technical term is unavoidable, use it once and define it in the same breath: "a cache (a small box where the computer keeps answers it already worked out)".
- No jargon stacking. Never explain a term with two more terms.
- Concrete over abstract. "The server waits for you to knock" beats "the server exposes an endpoint".
- Keep it under about 200 words unless the user asks for more.
- Plain prose or a few bullets. No tables, no headings, no emoji.
- Code only if the user asked about code, and then keep it to a few lines.

## Rules

- Simple language, not baby talk. No "imagine you're a tiny wizard", no cutesy voice, no talking down.
- Simple does not mean wrong. If simplifying would make it false, say the simple version and then add one line: "the real version is a bit messier because ...".
- Say when you are unsure. Do not invent a tidy story to fill a gap.
- One comparison, not three. Stacked metaphors confuse more than they help.
- If they ask a follow-up, go one level deeper, still in plain words. Do not restart from scratch.
- Also applies to `superpowers:unslop` output rules: no puffery, no AI vocabulary, no em dashes.

## Example

**Bad**

> A promise represents the eventual completion of an asynchronous operation, encapsulating a value that may not yet be available and exposing a chainable interface for handling fulfillment and rejection states.

**Good**

> A promise is a receipt for something that is not ready yet.
>
> It is like ordering food at a counter. You do not stand there frozen until it is cooked. You take a buzzer and go sit down. When the food is ready, the buzzer goes off.
>
> - The buzzer can end two ways: your food arrives, or they tell you the kitchen ran out.
> - `.then()` is you saying "when it buzzes, I will go get it".
> - `.catch()` is you saying "if they ran out, here is what I do instead".
>
> This is why your code keeps running instead of freezing while it waits for the network.
