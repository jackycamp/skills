---
name: asd-ste
description: "Makes an AI agent write its response in ASD-STE100-inspired Simplified Technical English. Use when the user invokes $asd-ste or /asd-ste, or asks the agent to answer, respond, explain, document, or write in ASD-STE100 or Simplified Technical English. Apply the style to the response itself; do not produce a before-and-after rewrite unless the user asks for one."
---

# ASD Simplified Technical English

Write the answer in clear, literal English that follows ASD-STE100 principles. Solve the user's task first. Apply this style to all explanatory prose.

## Apply the response mode

- Give the answer directly.
- Do not describe the STE rules that you used.
- Do not list violations or add a rewrite table unless the user asks for this analysis.
- Follow the user's required output format.
- Preserve code, commands, identifiers, field names, quotations, citations, and other exact text.
- Apply STE principles to the prose around exact text.

## Write clear sentences

- Use one word for one meaning. Use the same term each time for the same item or action.
- Prefer common and concrete words.
- Avoid idioms, metaphors, humor that depends on wordplay, and vague words.
- Use each word as one part of speech when this is practical.
- Use active voice. Name the actor when the actor is known and relevant.
- Use simple present, simple past, or simple future tense.
- Give only one instruction in each sentence.
- Keep an instruction at 20 words or fewer.
- Keep a descriptive sentence at 25 words or fewer.
- Keep the subject, verb, articles, and required objects explicit.
- Make each pronoun reference clear. Repeat the noun if a pronoun can have two meanings.
- Keep noun clusters to three words or fewer. Use a phrase or clause to show longer relationships.

## Organize the answer

- Put one topic in each paragraph.
- Use no more than six sentences in one paragraph.
- Use a list for three or more steps, options, or conditions.
- Put steps in the required order.
- Put a condition before the action that depends on it.
- Define an uncommon domain term at its first use.
- Use headings only when headings make the answer easier to scan.

## Preserve technical accuracy

- Preserve every fact, number, condition, exception, warning, and scope limit.
- Keep necessary technical terms. Define them instead of replacing them with less precise words.
- Do not shorten text if the shorter text changes the meaning.
- State uncertainty directly. Do not present an estimate or inference as a fact.
- Use `must` for a requirement. Use `can` for capability. Use `may` only for permission.

## Handle conflicts

Safety, accuracy, higher-priority instructions, and exact output requirements take priority over this style.

If a required sentence must exceed the length limit, keep the required meaning. Split the sentence when this does not change the meaning.

Do not claim that the answer has certified ASD-STE100 compliance. This skill applies the writing principles. It does not validate every word against the official ASD-STE100 dictionary.
