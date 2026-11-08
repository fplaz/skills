---
name: unslop
description: Edit prose to remove AI writing patterns (inflated vocabulary, filler, hedging, chatbot phrases, mannered style) without changing its meaning. Use when the user asks for it (unslop, "de-slop this", "make this read less like AI"), or when another skill such as create-prd, architect, think-deep, or learn applies it to its own draft before showing it. Do not run it on your own on ordinary replies.
---

# Unslop

Project rules in `AGENTS.md` (root and nested) take precedence over this skill when they conflict.

Edit text so it reads as written by someone who knows the subject and respects the reader's time. Keep the meaning and the intended tone. Shorter is usually better, but never drop a fact to get there.

## 1. Get the text

- **Called from another skill**: edit that skill's draft in place. Report nothing and hand the draft back.
- **A file path**: edit the file.
- **A Linear issue or doc**: read it, then show the edited text and ask before writing it back.
- **Pasted text, or nothing**: edit the pasted text, or the last thing you wrote in this conversation, and reply with the result.

## 2. Edit prose only

Leave these exactly as they are: code blocks and inline code, identifiers, file paths, commands, front matter, Mermaid, tables of data, links, and quoted material (someone else's words, error messages, issue text being cited).

Read `docs/glossary.md` if it exists. Glossary terms are fixed: never swap one for a synonym, and replace synonyms of a glossary term with the term itself.

If you cannot fix a sentence without guessing what it means, leave it and flag it.

## 3. Patterns

### Content

1. **Significance inflation.** "a pivotal step", "a key part of", "plays a vital role in", "marks a turning point". Say what it does.
2. **Promotional tone.** seamless, powerful, robust, cutting-edge, effortless, best-in-class. Replace with the property or the number, or cut it.
3. **Unsupported claims.** "Experts agree", "it is widely known", "studies show", and trailing "-ing" clauses that assert importance ("..., ensuring reliability", "..., highlighting the need for"). Cite the `path`, issue, commit, or number, or cut the claim.
4. **Generic conclusions.** "This lays a strong foundation", "the future looks bright". End on the specific next step or fact, or just end.

### Words

5. **Plain words.** use (not utilize, leverage), help (not facilitate), many (not numerous), if (not in the event that), also (not additionally). Cut AI favorites: delve, crucial, intricate, interplay, landscape, tapestry, testament, underscore, vibrant, enhance, foster, garner, showcase.
6. **Plain "is" and "has".** "serves as", "stands as", "acts as", "boasts", "features" usually mean "is" or "has".
7. **One name per thing.** Do not rotate synonyms for variety (user, customer, member, client in one paragraph). Pick one, preferably the glossary term, and repeat it.
8. **Concrete nouns over metaphors.** substrate, vector, locus, nexus, paradigm, north star, flywheel, wedge, bedrock, and "surface" or "scaffolding" used figuratively. Use the concrete word. Literal uses are fine: a Claude Code harness is a harness.

### Sentences

9. **State it directly.** "Not just X, but Y", "It's not X, it's Y", and "from X to Y" where X and Y are not two ends of a scale. Say the point, or list the items.
10. **Natural counts.** Do not force ideas into threes. Use as many as there are.
11. **Active voice.** "queries are validated" becomes "the compiler validates queries". Keep the passive only when the actor is unknown or does not matter.
12. **Strong verbs, few adverbs.** "significantly improves" becomes the measured change. "runs quickly" becomes "is fast", or the number.
13. **One idea per sentence.** If the reader has to reread to parse it, split it or drop clauses.
14. **Whole sentences.** No dropped articles, verbless fragments, arrows, or private abbreviations. "Bad date → exit 2, no write" becomes "A bad date exits with code 2 and writes nothing."
15. **Literal over figurative.** No aphorisms, rhetorical fragments, personified code ("the plan holds it"), or figurative verbs ("rides along"). Say what happens.
16. **Specific to this project.** If a sentence could appear unchanged in another project's docs, it says nothing about this one. Make it name the mechanism, the file, or the number, or cut it.

### Filler and hedging

17. **Filler.** "In order to" becomes "to". "Due to the fact that" becomes "because". Cut "It is important to note that", "It's worth mentioning", "Basically".
18. **Hedging.** One qualifier at most, and only when the uncertainty is real. "could potentially be argued that it might" becomes "may".

### Chat leftovers

19. **Chatbot phrases.** "I hope this helps", "Let me know if", "Certainly!", "Great question", "You're absolutely right". Cut them.

### Formatting

20. **No em dashes.** Use a period or a comma. Parentheses are fine for a short aside.
21. **Colons only before a list, an example, or a definition.** Not as a connector in the middle of a sentence.
22. **Little bold.** Bold a lead-in label or a real warning. Not every product name or key word.
23. **No restating labels.** "**Performance:** Performance improved..." becomes a plain sentence. A bold lead-in that ends in a period and is followed by new detail ("**Scope.** Prose only.") is fine.
24. **Sentence case headings.** "How invites expire", not "How Invites Expire".
25. **No decorative emoji** in headings or bullets.
26. **Straight quotes** (`"` and `'`), not curly ones.

## 4. Report

Unless called from another skill, list the main kinds of change in a few lines (for example "cut 6 filler phrases, replaced 3 synonyms of Lease with Lease") and any sentences you flagged. Do not commit.
