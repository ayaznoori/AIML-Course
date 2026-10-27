# Lecture Script: How AI and LLMs Work with Prompts
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Lecture Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | AI, Machine Learning, and Generative AI — What Each Term Covers | 12 min |
| 3 | Next-Token Prediction | 18 min |
| 4 | Tokenization | 15 min |
| 5 | The Context Window | 13 min |
| 6 | Temperature | 13 min |
| 7 | Fluent but Wrong | 12 min |
| 8 | The Four Elements of a Well-Formed Prompt | 14 min |
| 9 | Lecture Summary and Recap | 5 min |

---

## Learning Objectives

By the end of this lecture, learners will be able to:

1. Compare AI, machine learning, and generative AI by what each term covers
2. Describe next-token prediction as the mechanism that produces an LLM response
3. Describe how a tokenizer splits text into subword tokens
4. Explain what a context window limits and what counts against it
5. Predict how raising temperature changes the variability of an output
6. Explain why a model can produce fluent text that is factually wrong
7. State the four elements of a well-formed prompt: task, context, format, and constraints

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience is meeting LLM internals for the first time. Do not assume programming experience or any machine learning background. The hook should present three behaviors that look unrelated and puzzling, then promise that one mechanism explains all three. Everything that follows returns to that mechanism. Open with the question and wait for real answers before continuing.

**[Script:]**

"Have you ever asked an AI tool a question, received an answer that sounded completely confident and polished, and then discovered part of it was simply wrong? Or asked the same question twice and received two different answers? Take a moment and think of an example.

Here is a scenario that combines both. A manager pastes a long quarterly report into an AI assistant and asks for a summary. The summary reads beautifully. But it includes a growth figure that appears nowhere in the report. She is puzzled, so she asks again. This time the summary is organized differently and leaves out a section she cared about. A third attempt cuts off partway through the report as if the end was never read.

Three behaviors: fluent but wrong, different every time, and sometimes ignoring part of the input. These look like three separate problems. They are not. All three follow from how a language model actually produces text, and once you understand that mechanism, these behaviors stop being mysterious and start being predictable.

Today we build that understanding step by step. We start by sorting out what the terms AI, machine learning, and generative AI actually mean. Then we look at the core mechanism, predicting one small piece of text at a time. From there we see how text is chopped into pieces called tokens, why a model can only handle a limited amount of text at once, and how a single setting called temperature controls how varied the output is. Then we confront the biggest risk head-on: why fluent text can be factually wrong. We finish with the practical payoff: the four elements of a well-formed prompt, and why each one exists, given everything you will have learned about how the model works."

---

## Block 2 — AI, Machine Learning, and Generative AI: What Each Term Covers

### 2A — Three Terms, Three Different Scopes

**[Script:]**

"These three terms are used almost interchangeably in everyday conversation, and that causes real confusion. They are not synonyms. Each one covers a different amount of ground, and the clearest way to see this is as nested circles, each one sitting inside the one before it."

> 🎯 **Instructor Note:** Draw this on the board and keep it visible for the whole lecture. Build it from the outside in, one circle at a time, naming what each covers before drawing the next.

```
+-----------------------------------------------------+
| ARTIFICIAL INTELLIGENCE (AI)                        |
| Any system doing tasks that normally need human     |
| intelligence — including hand-written rule systems  |
|   +---------------------------------------------+   |
|   | MACHINE LEARNING (ML)                       |   |
|   | Systems that LEARN patterns from data       |   |
|   | instead of following hand-written rules     |   |
|   |   +-------------------------------------+   |   |
|   |   | GENERATIVE AI                       |   |   |
|   |   | ML models that CREATE new content   |   |   |
|   |   | (text, images, code)                |   |   |
|   |   +-------------------------------------+   |   |
|   +---------------------------------------------+   |
+-----------------------------------------------------+
```

**[Script:]**

"Artificial intelligence is the broadest term. It covers any system built to perform a task that would normally require human intelligence: playing chess, recognizing speech, recommending a movie. Critically, it includes systems where a programmer wrote every rule by hand. A chess program that evaluates moves using rules its authors wrote is AI, even though it never learned anything from data.

Machine learning is a subset of AI. Instead of a programmer writing the rules, the system is shown many examples and learns the patterns itself. A spam filter that was trained on thousands of emails labeled spam or not spam is machine learning. Nobody wrote a rule saying 'the word lottery means spam'; the system found that pattern in the examples.

Generative AI is a subset of machine learning. It refers to models that create new content, such as text, images, audio, or code, rather than only assigning a label or a score to something that already exists. Today's generative AI models are built using machine learning. A large language model, or LLM, is generative AI that works with text, and it is the focus of everything today."

---

### 2B — Sorting Examples

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Show the three systems below one at a time. For each, ask the room to place it in the diagram: AI only, ML, or generative AI. Take several answers aloud before revealing. Then ask: "Is every machine learning system also generative AI?" Answer: no. The spam filter is machine learning but does not create new content. Ask the reverse: "Is every generative AI system also AI?" Answer: yes, it sits inside both larger circles.

**Demo 1 — Placing systems in the diagram (whiteboard-friendly)**

```
System A: A chess program using hand-written move-scoring rules
  → AI, but NOT machine learning (no learning from data)

System B: A spam filter trained on labeled example emails
  → AI and machine learning, but NOT generative (it labels, not creates)

System C: A chatbot that drafts a new email from your instructions
  → AI, machine learning, AND generative AI
```

**[Script:]**

"Notice the pattern: each circle adds a requirement. System A only meets the broad definition. System B also learned from data. System C also produces new content. The more specific the term, the fewer systems it describes. When someone says 'we use AI,' it can mean almost anything. When someone says 'we use a large language model,' you know it is generative, learned from data, and works with text."

> 🎯 **Instructor Note:** Name the common confusion directly: "AI," "machine learning," and "LLM" are not three words for the same thing. If a learner uses them interchangeably, gently redirect them to the circles: which circle are you actually describing?

**Recap of Block 2 before moving on:**

- AI is the broadest term: any system performing a task that normally needs human intelligence, including systems built entirely from hand-written rules
- Machine learning is a subset of AI where the system learns patterns from data rather than following rules a programmer wrote
- Generative AI is a subset of machine learning where models create new content such as text, images, or code
- An LLM is generative AI for text; each narrower term describes fewer systems than the broader one

---

## Block 3 — Next-Token Prediction

### 3A — The Autocomplete You Already Know

**[Script:]**

"You already use a small version of the mechanism behind an LLM. When you type a message on your phone, the keyboard suggests the next word. You type 'see you', and it offers 'soon', 'later', 'tomorrow'. The keyboard is predicting what is likely to come next, based on patterns in text.

A large language model does exactly this, at a vastly greater scale and sophistication. Its entire job, the single thing it does, is this: given some text, predict what small piece of text most plausibly comes next. Everything an LLM appears to do, such as answering questions, writing code, or translating, is built on repeating that one operation."

---

### 3B — How a Whole Response Gets Produced

**[Script:]**

"One prediction produces one small piece of text, called a token, which for now you can think of as roughly a word or part of a word. So how does an entire paragraph appear? The model repeats the process in a loop."

> 🎯 **Instructor Note:** Write this loop on the board and keep it visible. It is the central idea of the lecture.

```
1. Read ALL the text so far (your prompt + anything already generated)
2. Calculate a probability for every possible next token
3. Choose one token
4. Add that token to the text
5. Go back to step 1 — until the model predicts a special "end" token
```

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Show only the first row of the demo and ask: "After the model produces 'The sky is blue', what do you think it does next? Does it stop, or does it do something else?" Answer: it runs the exact same process again, using the longer text, and might predict a period, more words, or the end token. Then reveal the remaining rows.

**Demo 2 — Generating text one token at a time (whiteboard-friendly)**

```
Probabilities below are illustrative, not from a real model.

Step 1  Text so far: "The sky is"
        Candidates: " blue" 55% | " clear" 15% | other 30%     → picks " blue"
Step 2  Text so far: "The sky is blue"
        Candidates: "." 50% | " and" 15% | other 35%           → picks "."
Step 3  Text so far: "The sky is blue."
        Candidates: <end> 70% | " The" 20% | other 10%         → picks <end>
```

**[Script:]**

"Look at what happened. At each step the model looked at everything produced so far, assigned a probability to every possible next token, and chose one. At step 3 the best candidate was a special end token, which means 'I am done', and that is how the model decides to stop. The response was not looked up in a database, and it was not planned in advance as a complete sentence. It was built one token at a time, with each choice depending on all the choices before it.

Two consequences follow, and you will see both again before we finish. First, the model's own earlier output becomes part of the input for every later step, so an early choice shapes everything after it. Second, the model is always choosing among plausible continuations. It is not checking whether a continuation is true."

> 🎯 **Instructor Note:** Name the common confusion: learners often picture the model retrieving a stored answer, the way a search engine returns a stored page. Correct it directly: an LLM generates its response token by token; it does not fetch a pre-written answer. Ask: "Where did the training come in?" Answer: during training, the model was shown enormous amounts of text and repeatedly adjusted to get better at predicting the next token; that is how it learned patterns of language, facts as they appear in text, and styles of writing.

**Recap of Block 3 before moving on:**

- An LLM's core operation is predicting the most plausible next piece of text given everything so far
- A full response is produced by repeating that prediction in a loop: predict, choose, add to the text, repeat
- The model stops when it predicts a special end token
- Each token depends on all earlier text, including the model's own output, so early choices shape later ones
- The model generates a response rather than retrieving one, and it selects plausible continuations rather than verified facts

---

## Block 4 — Tokenization

### 4A — What a Token Actually Is

**[Script:]**

"I have been saying 'token' as if it meant 'word.' It does not, and the difference matters. Before a model can process text, a component called a tokenizer splits the text into pieces called tokens. Each token is then represented as a number, because the model works with numbers, not letters. The model never sees your text as letters or words; it sees a sequence of token numbers.

Tokens are usually subwords, meaning pieces of words. Common words are often a single token. Less common words are split into several smaller pieces. Spaces are typically attached to the front of the token that follows them."

---

### 4B — Seeing a Tokenizer Split Text

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Write the sentence "Tokenization is unbelievably fun" on the board. Ask: "How many words is this? How many tokens do you think a tokenizer produces: fewer, the same, or more than the word count?" Let learners commit to a guess, then reveal. Also ask: "If I invent a word like 'flurbotronic', will it be one token or several?" Answer: several, since an invented word is not in the tokenizer's vocabulary as a unit and must be built from smaller pieces.

**Demo 3 — Subword splitting (whiteboard-friendly)**

```
Splits are illustrative; exact splits differ between tokenizers.

Text:    "Tokenization is unbelievably fun"
Tokens:  ["Token", "ization", " is", " un", "believ", "ably", " fun"]
Count:   4 words  →  7 tokens

Text:    "flurbotronic"   (an invented word)
Tokens:  ["fl", "urb", "ot", "ronic"]
Count:   1 word  →  4 tokens
```

**[Script:]**

"Four words became seven tokens. Short, common words like 'is' and 'fun' stayed whole. The longer words were broken into familiar fragments. The invented word became four tokens, because the tokenizer fell back to smaller pieces it does know.

Why not just use whole words? A vocabulary containing every word in every language, plus every name, typo, and technical term, would be unmanageably large and would still fail on new words. Why not use single characters? Then even short texts become very long sequences. Subwords are the compromise: a fixed vocabulary of reusable pieces that can build any word, including ones the model has never seen."

> 🎯 **Instructor Note:** Give the planning rule of thumb, and mark it clearly as an approximation: in English, one token is roughly four characters, or about three quarters of a word. Then state the practical consequences. Tokens, not words, are what models count, what limits are measured in, and what usage is typically billed by. Also mention that text in some other languages, and unusual content such as long numbers or code, often needs more tokens for the same meaning. Optionally ask: "If a model sees tokens rather than letters, why might it struggle with a question like 'how many times does the letter r appear in strawberry'?" Answer: it never directly sees the individual letters, only token pieces, so counting letters is not something its representation makes easy.

**Recap of Block 4 before moving on:**

- A tokenizer splits text into tokens before the model processes it, and each token becomes a number
- Tokens are usually subwords: common words are often one token, rarer words are split into several pieces
- A token is not a word; four words can easily be seven tokens, and invented or rare words split into many pieces
- Subword tokenization lets a fixed vocabulary build any word, including words the model has never seen
- Roughly four characters of English per token is a planning estimate; exact splits differ between tokenizers

---

## Block 5 — The Context Window

### 5A — A Fixed Amount of Text at a Time

**[Script:]**

"Recall the loop from earlier: at every step the model reads all the text so far. But 'all the text' has a ceiling. The context window is the maximum number of tokens a model can take into account in a single request. It limits how much text the model can work with at once.

The part that surprises people is what counts against it. The window is not only your latest message. Everything in the request counts: any hidden instructions the application gave the model, the entire conversation history so far, any document you pasted in, and the response the model is currently generating. Input and output share the same budget."

> 🎯 **Instructor Note:** Write this list on the board.

```
Counts against the context window:
  - System instructions set by the application
  - The whole conversation history so far
  - Any documents or text you paste in
  - Your current message
  - The model's response as it is being written
```

---

### 5B — Doing the Budget Arithmetic

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Show the budget below but cover the last line. Ask: "How many tokens are left for the model's answer? Could it write a 1,000-token answer?" Let learners subtract. Answer: 500 tokens are left, so a 1,000-token answer cannot fit.

**Demo 4 — A context window budget (whiteboard-friendly)**

```
Context window size:          8,000 tokens   (input + output together)
System instructions:            500
Conversation history:         5,000
Pasted document:              2,000
Used before any answer:       7,500
Left for the response:          500
```

**[Script:]**

"The window is 8,000 tokens in total. Instructions, history, and the pasted document already use 7,500. Only 500 remain for the answer, so a long response would be cut short or the request would fail. Window sizes differ between models and change over time, so check the documentation for the model you are using; the principle does not change.

This also explains one of the puzzles from the opening. When a conversation or document is too long for the window, the application must drop or shorten something, or the request fails. Whatever does not fit in the window is simply not available to the model for that response. The manager's third summary that seemed to ignore the end of the report is a typical symptom."

> 🎯 **Instructor Note:** Name the common confusion: the context window is not long-term memory. The model does not remember earlier conversations on its own. Within a conversation, the application sends the history back with each request, and that history counts against the window. Ask: "If a detail from the start of a very long conversation no longer seems to influence the model, what is the likely reason?" Answer: that early text may have been shortened or removed to fit the window, so the model no longer receives it.

**Recap of Block 5 before moving on:**

- The context window is the maximum number of tokens a model can consider in a single request
- Input and output share it: instructions, conversation history, pasted documents, your message, and the response all count
- Whatever does not fit is not available to the model for that response
- The window is not long-term memory; history must be sent again with each request
- Window sizes vary by model and change over time, so check the documentation for the model in use

---

## Block 6 — Temperature

### 6A — How the Model Chooses Among Candidates

**[Script:]**

"Return to the loop. At each step the model has a probability for every candidate token. Step 3 of the loop says 'choose one token.' How is that choice made? One option is to always take the single most probable token. Another is to pick randomly, weighted by probability, so that likely tokens are chosen often and unlikely tokens are chosen occasionally.

Temperature is a setting that controls this. Think of the phone keyboard again. At low temperature, the model behaves like a keyboard that always takes the top suggestion. At high temperature, it behaves like one that sometimes takes the third or fourth suggestion instead."

> 🎯 **Instructor Note:** Write this on the board and keep it visible.

```
Low temperature   → probabilities sharpened → top candidate wins almost always
                    → consistent, repeatable output

High temperature  → probabilities flattened → less likely candidates get chosen
                    → more varied output, with more risk of odd or off-target text
```

---

### 6B — Predicting the Effect

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Show the prompt and the low-temperature column only. Ask: "If I run this prompt ten times at low temperature, what do you expect to see? Now, if I raise the temperature and run it ten times again, will the variability across the ten outputs go up or go down?" Answer: variability goes up. Have learners say it aloud before revealing the high-temperature column.

**Demo 5 — The same prompt at two temperatures (whiteboard-friendly)**

```
Prompt: "Name a color."   (probabilities illustrative)

Token       Low temperature     High temperature
" blue"          90%                 40%
" green"          6%                 25%
" red"            3%                 20%
" teal"           1%                 15%

Ten runs, expected pattern:
  Low:  almost every run says "blue"
  High: a mix of blue, green, red, and teal
```

**[Script:]**

"Same model, same prompt, same knowledge. Only the temperature changed. At low temperature, 'blue' dominates and nearly every run agrees. At high temperature, the probabilities flatten and the other colors appear regularly. This is the second puzzle from the opening: the manager's two summaries differed because the model was sampling among plausible continuations, and every small difference early in the response changed what came after.

Use this to choose a setting for the task. Low temperature suits work that should be consistent: extracting information, classifying, or answering from a document. Higher temperature suits work that benefits from variety: brainstorming names, drafting alternative phrasings. The exact range of the setting differs between tools, so check the one you use."

> 🎯 **Instructor Note:** Name two common confusions. First, temperature is not an intelligence or quality dial: raising it does not make the model smarter or more creative in any deeper sense; it only makes less likely tokens more likely to be chosen, which can produce interesting variety and can just as easily produce errors. Second, a low temperature makes output more repeatable, but it does not make the output more factually correct. A model can consistently repeat the same wrong answer. Also note that even at very low settings, some systems may not return word-for-word identical output every time, so treat low temperature as 'more consistent,' not 'guaranteed identical.'

**Recap of Block 6 before moving on:**

- At each step the model chooses one token from a probability distribution, and temperature controls how that choice is made
- Low temperature sharpens the distribution, so the most likely token wins almost always and output is more consistent
- Raising temperature flattens the distribution, so less likely tokens are chosen more often and output variability increases
- Low temperature suits consistent tasks such as extraction; higher temperature suits tasks that benefit from variety
- Temperature changes variability, not intelligence, and lowering it does not make answers more factually correct

---

## Block 7 — Fluent but Wrong

### 7A — The Third Puzzle

**[Script:]**

"The last puzzle from the opening is the most important one: the summary that contained a number appearing nowhere in the report. The technical term for this is a hallucination, a response that sounds plausible and confident but is not supported by facts or by the source it was given.

Everything you now know explains why this happens. The model's job is to choose a plausible next token. It was trained to predict text, not to verify claims. Nothing in the basic loop checks a statement against reality or against a source. Fluency is a property of the pattern of the text. Truth is a separate property that the mechanism does not enforce."

---

### 7B — A Concrete Failure

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Show the request below. Ask: "The model has no list of real studies it is checking against. What will it tend to produce when asked for exact citations, and why might that be risky?" Guide toward: it will tend to produce text shaped like a citation, because the pattern of authors, year, title, and journal is extremely predictable, whether or not a matching study exists. Then ask: "Does a confident, well-formatted tone tell you the answer is correct?" Answer: no.

**Demo 6 — A plausible-looking citation (whiteboard-friendly)**

```
Request:  "Give me three peer-reviewed studies, with authors, year, and
           journal, on the effect of four-day work weeks on support
           response times."

Output:   Three entries in perfect citation format.

Check:    Search each title in a library database or search engine.

Risk:     Some entries may not exist. The model produced text shaped
          like a citation, which is easy to predict, without verifying
          that any matching study exists.
```

**[Script:]**

"The danger is not that the output looks sloppy. The danger is that it looks excellent. The areas of highest risk are exactly the ones that feel authoritative: precise numbers, dates, names, quotations, citations, and details about obscure or very recent topics. A model's training data also has a cutoff, so it may be unaware of recent events unless it is given current information.

What can you do? Treat output as a draft to verify, not a fact to trust, especially anything specific. Put the real source text into the prompt and tell the model to rely on it, so it is drawing on actual content rather than patterns. And tell it explicitly that it may say it does not know instead of guessing. These are prompt-writing techniques, and they are exactly where the last block of today leads."

> 🎯 **Instructor Note:** Reinforce the earlier confusion point about temperature: lowering the temperature makes the model more consistent, not more correct. Ask: "If a model repeatedly gives the same wrong date at a low temperature, which two earlier ideas explain it?" Answer: it is choosing the most probable continuation each time, and probability reflects patterns in text, not verification of truth. Also name the broader confusion: fluent writing is not evidence of understanding or accuracy.

**Recap of Block 7 before moving on:**

- A hallucination is output that sounds plausible and confident but is not supported by facts or by the provided source
- It happens because the mechanism selects plausible next tokens and does not check claims against reality
- Fluency and correctness are separate; confident tone is not evidence of accuracy
- Highest-risk content includes specific numbers, dates, names, quotations, citations, and recent or obscure topics
- Mitigations include verifying specifics, supplying real source text, and permitting the model to say it does not know

---

## Block 8 — The Four Elements of a Well-Formed Prompt

### 8A — From Mechanism to Practice

**[Script:]**

"A prompt is the text you give the model, and it is the starting point of the loop. Every token the model generates depends on it. A vague prompt leaves enormous room for plausible continuations, many of which are not what you wanted. A specific prompt narrows the range of likely continuations toward the one you need.

A well-formed prompt has four elements."

> 🎯 **Instructor Note:** Write these four elements on the board and keep them visible through the end of the lecture.

```
TASK         What you want done — a clear action
CONTEXT      Background the model needs — situation, audience, source text
FORMAT       The shape of the output — structure, length, layout
CONSTRAINTS  The rules and limits — what to include, avoid, or rely on
```

**[Script:]**

"The task is the action you want performed, stated specifically: summarize, classify, rewrite, compare. Context is what the model needs to know that it cannot guess: who the output is for, what situation it is used in, and any source text it should work from. Format is the shape of the answer: bullets, a table, a word count, a specific layout. Constraints are the boundaries: what must be included, what must be avoided, what sources are allowed, and what to do when information is missing."

---

### 8B — Weak Versus Well-Formed

**Predict before the demo: What will happen?**

> 🎯 **Instructor Note:** Show the weak prompt first. Ask: "Which of the four elements are present, and which are missing? What range of different outputs could this prompt plausibly produce?" Let learners name the missing elements before showing the improved version.

**Demo 7 — A weak prompt and a well-formed prompt (whiteboard-friendly)**

```
WEAK:
"Summarize this email."

WELL-FORMED:
TASK:        Summarize the customer email below.
CONTEXT:     The reader is a support manager with 30 seconds who must decide
             whether to escalate. [email text pasted here]
FORMAT:      Three bullet points, then one line: "Escalate? Yes or No".
CONSTRAINTS: Under 60 words. Use only information in the email. If the
             email is unclear, say so instead of guessing.
```

**[Script:]**

"The weak prompt contains only a task, and a vague one. The well-formed version supplies all four. Notice how each element connects to what you learned today. The context supplies real source text and tells the model who the reader is, which gives it actual content to draw on, reducing the room for hallucination. The format controls the structure and length, so the response stays manageable inside the token budget. The constraints include 'use only information in the email' and 'say so instead of guessing,' which directly address the fluent-but-wrong problem. And all four together narrow the probable continuations toward the one you need, which also reduces the output variability you saw at higher temperatures."

> 🎯 **Instructor Note:** Name the common confusion: a longer prompt is not automatically a better prompt. The goal is not more words but the right four elements, stated clearly. Also acknowledge that format and constraints can overlap in practice; a rule of thumb is that format describes the shape of the answer and constraints describe the rules and limits it must obey. Do not spend time on exact boundaries. What matters is that all four ideas are present.

---

### 8C — Practice

**[Script:]**

"Now you do it. I will give you a prompt, and you identify which words belong to each element. Then you will rewrite a weak prompt."

> 🎯 **Instructor Note:** Run this as a short individual or pair exercise, three to four minutes, then discuss answers. Write both prompts on the board.

```
Exercise 1 — Label the elements:
"Write a product description for our water bottle for parents of young
children. It is insulated and dishwasher safe. Use two short paragraphs,
a friendly tone, and do not mention price."

Exercise 2 — Rewrite so all four elements are present:
"Write something about our event."
```

> 🎯 **Instructor Note:** Answers for Exercise 1: Task is "write a product description"; context is "for our water bottle," "for parents of young children," and "insulated and dishwasher safe"; format is "two short paragraphs"; constraints are "friendly tone" and "do not mention price." Tone could reasonably be treated as either format or constraint; accept either with a reason. For Exercise 2, there is no single correct answer. Look for a rewrite that names a specific action, supplies event details and an audience, states a length or structure, and sets at least one limit. Ask a few learners which element they added and what the weak version had left open.

**Recap of Block 8 before moving on:**

- A prompt is the starting text of the generation loop, and a specific prompt narrows the likely continuations toward what you need
- A well-formed prompt has four elements: task, context, format, and constraints
- Task is the specific action; context is the background and source material the model cannot guess; format is the shape of the output; constraints are the rules and limits
- Supplying real source text and instructing the model to rely on it addresses fluent-but-wrong output
- A better prompt is one with the right four elements, not simply a longer one

---

## Block 9 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as rapid active recall. Ask each question and take answers before confirming. "Which is the broadest term: AI, machine learning, or generative AI? What is the one operation an LLM repeats to produce a response? Is a token the same as a word? What counts against the context window? What happens to output variability when temperature rises? Why can a fluent answer be wrong? Name the four elements of a prompt." Return to the manager's three puzzles and ask learners to explain each in one sentence using today's ideas.

**AI, Machine Learning, and Generative AI**

- AI is the broadest term, machine learning is the subset that learns patterns from data, and generative AI is the subset of machine learning that creates new content
- An LLM is generative AI for text

**Next-Token Prediction**

- An LLM repeatedly predicts the most plausible next token given all text so far, adds it, and repeats until it predicts an end token
- A response is generated, not retrieved, and each token depends on everything before it, including the model's own output

**Tokenization**

- A tokenizer splits text into subword tokens, and each token becomes a number the model processes
- A token is not a word: common words are often one token, and rarer or invented words split into several

**The Context Window**

- The context window limits how many tokens a model can consider in one request, and input and output share it
- Instructions, history, pasted documents, your message, and the response all count; what does not fit is unavailable to the model

**Temperature**

- Temperature controls how the next token is chosen from the probability distribution
- Raising it increases output variability; lowering it makes output more consistent, though not more correct

**Fluent but Wrong**

- The mechanism selects plausible text and does not verify claims, so fluent output can be factually wrong
- Verify specifics, supply real source text, and permit the model to say it does not know

**The Four Elements of a Well-Formed Prompt**

- Task, context, format, and constraints
- Each element narrows the range of plausible continuations toward the output you need

**Why All of This Matters Together**

- Every behavior from the opening scenario traces back to one mechanism: a model that predicts the next token, one at a time, from a limited window of text; fluent but wrong output comes from choosing plausible rather than verified text, different answers on repeated attempts come from sampling among candidates, and ignored input comes from the finite window; knowing this, a well-formed prompt is not a magic phrase but a deliberate way of steering that mechanism with a clear task, the right context, a defined format, and firm constraints, and that understanding is what lets you use these tools effectively while knowing exactly where to verify their output

---

*End of script.*
