# How AI and LLMs Work with Prompts

## What You Will Learn

AI chat tools are now used to draft emails, summarise study material, and explain difficult topics in a few seconds. The same tool can give a useful answer to one person and a poor answer to another, depending on how the request is written. This lecture explains what happens inside the tool after you press Enter, so that you can write requests that give dependable results.

By the end of this lecture, you will be able to:

- Compare **AI**, **machine learning**, and **generative AI** by what each term covers.
- Describe **next-token prediction** as the mechanism that produces an LLM response.
- Describe how a **tokenizer** splits text into subword tokens.
- Explain what a **context window** limits and what counts against it.
- Predict how raising the **temperature** changes the variability of an output.
- Explain why a model can produce fluent text that is factually wrong.
- State the four elements of a well-formed prompt: **task**, **context**, **format**, and **constraints**.

The lecture moves from the big picture to the details. It starts with the terms AI, machine learning, and generative AI, then opens up the LLM step by step, and ends with the skill of writing a good prompt. Each topic builds on the one before it, so read them in order.

---

## AI, Machine Learning, and Generative AI

People often use the words AI, machine learning, and generative AI as if they mean the same thing. They do not, because each term covers a different amount of ground. Knowing how they fit together helps you place every later topic correctly.

### Artificial Intelligence (AI)

- **Official Definition:** Artificial Intelligence is the field of building computer systems that perform tasks which normally need human intelligence, such as understanding language, making decisions, or recognising patterns.
- **In Simple Words:** AI is the big umbrella for any machine that appears to "think" or "decide" in a useful way.
- **Real-Life Example:** A ticket-booking chatbot that shows a fixed menu and replies with pre-written answers is a simple form of AI.
- AI is **not one single technology**. It includes simple programs that follow hand-written rules as well as advanced systems that learn from data.
- **Common doubt:** Not every AI tool is built on the same technology as a modern chat assistant.

Within this big umbrella, one approach became very powerful because it removed the need to write every rule by hand. That approach is machine learning.

### Machine Learning (ML)

- **Official Definition:** Machine Learning is a branch of AI in which a computer learns patterns from data instead of following rules written by a programmer for every situation.
- **In Simple Words:** You show the computer many examples, and it works out the pattern on its own.
- **Real-Life Example:** A food-delivery app estimates your delivery time by learning from thousands of past orders instead of using one fixed formula.
- ML is **needed** because some problems have too many cases to write rules for, such as judging whether a payment looks suspicious.
- Most ML systems **predict or classify**. They give a number, a label, or a decision, and they do not create new content.

Some ML systems go one step further. Instead of only labelling or predicting, they produce something new.

### Generative AI

- **Official Definition:** Generative AI is a branch of machine learning that creates new content, such as text, images, audio, or code, based on patterns learned from large amounts of training data.
- **In Simple Words:** It is AI that writes or draws something new instead of only giving a label or a number.
- **Real-Life Example:** A chat assistant that writes a leave letter from a short request, or an image tool that creates a poster from a description.
- Every generative AI system uses machine learning, but **most machine learning systems are not generative**.
- **Common doubt:** Generative AI does not copy-paste a stored answer. It builds a fresh output from learned patterns.

### Large Language Model (LLM)

- **Official Definition:** A Large Language Model is a generative AI model trained on very large amounts of text so that it can understand and generate human language.
- **In Simple Words:** An LLM is a program that has "read" a huge amount of text and can continue or answer in a convincing way.
- **Real-Life Example:** Imagine a student who has read an entire library and can complete almost any sentence you start.
- The word **Large** refers to the huge amount of training text and the huge number of parameters inside the model.
- The words **Language Model** mean that it works with text, so an LLM is one kind of generative AI.

Two more terms appear throughout this lecture, so it helps to define them now.

- **Model**
  - **Official Definition:** A model is a program that has learned patterns from data and can apply them to new input.
  - **In Simple Words:** It is the trained "brain" that does the work after learning is finished.
  - **Real-Life Example:** A chef who has cooked thousands of dishes and now cooks from experience instead of reading a recipe every time.
- **Training**
  - **Official Definition:** Training is the process of adjusting a model using large amounts of data until it becomes good at its task.
  - **In Simple Words:** It is the practice phase before the model is used.
  - **Real-Life Example:** Preparing for an exam by solving thousands of previous year question papers.
- **Parameters**
  - **Official Definition:** Parameters are the internal numbers of a model that are adjusted during training and store what the model has learned.
  - **In Simple Words:** They are the settings inside the model that hold its knowledge.
  - **Real-Life Example:** The positions of millions of knobs on a giant mixing board, set after long practice.

### How the Terms Fit Together

```text
Artificial Intelligence (widest: any system doing tasks that need human intelligence)
  └── Machine Learning (learns patterns from data)
        └── Generative AI (creates new content)
              └── Large Language Models (generate text)
```

| Term | What it covers | Typical output | Example |
|---|---|---|---|
| **AI** | Any system that performs tasks needing human intelligence, whether it uses fixed rules or learning | A decision or a response | Ticket-booking chatbot with a fixed menu |
| **Machine Learning** | AI systems that learn patterns from data | A prediction or a label | Delivery-time estimate from past orders |
| **Generative AI** | ML systems that create new content | New text, image, audio, or code | Assistant that writes a leave letter |

### Activity 1: Sort the Examples

Read each example below and decide which term is the **most specific** one that fits: AI (rule-based), Machine Learning, or Generative AI. Write your answers on paper before checking the key.

1. A rule-based assistant in which a developer wrote every reply by hand.
2. A system that learns from past house sales to predict the price of a new house.
3. A tool that writes a short poem when you type a topic.
4. A tool that creates a new image from a text description.
5. A system that learns from thousands of labelled photos to decide whether a leaf is healthy or diseased.
6. An assistant that summarises a PDF that you upload.

**Check your answers:**

- 1 is **AI (rule-based)**, because it follows hand-written rules and does not learn from data.
- 2 and 5 are **Machine Learning**, because they learn from data to predict or label, and they create nothing new.
- 3, 4, and 6 are **Generative AI**, because each one creates new content. Items 3 and 6 are also built on LLMs because they work with text.

---

## How an LLM Generates a Response: Next-Token Prediction

You now know that an LLM is a generative AI model for text. The next question is what it actually does when you send it a message. The answer is a single repeated step called next-token prediction.

### Next-Token Prediction

- **Official Definition:** Next-token prediction is the process in which a language model reads the text so far, calculates a probability for every possible next token, and selects one.
- **In Simple Words:** It is a very advanced version of the word suggestions on your phone keyboard. The model keeps asking, "What small piece of text should come next?"
- **Real-Life Example:** If someone says "Atithi Devo ...", most people complete it with "Bhava" because they have heard the phrase many times. An LLM does something similar, but across billions of phrases.
- A **token** is a small piece of text. The next section explains exactly how text is split into tokens, so for now you can think of a token as a word or part of a word.
- A **probability** here is a score for each possible next token, which you can read as a percentage chance.

### The Steps Behind Every Response

1. Your message is converted into tokens.
2. The model calculates a probability for **every token** it knows, based on all the text so far.
3. One token is selected from those probabilities.
4. The selected token is added to the end of the text.
5. Steps 2 to 4 repeat until the model selects a special stop token or reaches a length limit.

### Common Doubts

- **Does the model write the whole answer at once?** No. It produces **one token at a time**, and every selected token becomes part of the input for the next step.
- **Does the model look up a stored answer?** No. It calculates probabilities from patterns learned during training, and it does not browse the internet unless the tool has a search feature connected to it.
- **Why can the same question give different answers?** The selection step involves a choice between likely tokens. The temperature section later in this lecture explains how that choice is controlled.

### Next-Token Prediction in Code

The program below imitates the loop using a tiny table of probabilities. It always picks the most likely next word, and it stops when it picks the stop marker `<end>`.

```python
# This table is a tiny pretend "model": for each word, it lists the possible next words with their probabilities
next_word_table = {  # start of the table
    "I": {"love": 0.6, "like": 0.4},            # after "I", the word "love" is more likely than "like"
    "love": {"chai": 0.7, "cricket": 0.3},      # after "love", "chai" is more likely than "cricket"
    "like": {"tea": 1.0},                       # after "like", only "tea" is possible in this tiny table
    "chai": {"every": 0.55, "<end>": 0.45},     # after "chai", the model may continue or stop
    "cricket": {"<end>": 1.0},                  # after "cricket", the sentence always ends
    "tea": {"<end>": 1.0},                      # after "tea", the sentence always ends
    "every": {"morning": 0.8, "evening": 0.2},  # after "every", "morning" is more likely than "evening"
    "morning": {"<end>": 1.0},                  # after "morning", the sentence always ends
    "evening": {"<end>": 1.0},                  # after "evening", the sentence always ends
}  # end of the table

sentence = ["I"]                                # start the sentence with the word "I"
current_word = "I"                              # remember the latest word, because the next word depends on it

while current_word != "<end>":                  # keep predicting until the model chooses the stop marker
    options = next_word_table[current_word]     # get the possible next words and their probabilities
    next_word = max(options, key=options.get)   # pick the word with the highest probability
    print("After", repr(current_word), "the model picks", repr(next_word), "with probability", options[next_word])  # show each step
    current_word = next_word                    # the chosen word becomes the new latest word
    if current_word != "<end>":                 # check that we did not just pick the stop marker
        sentence.append(current_word)           # add the chosen word to the sentence

print("Final sentence:", " ".join(sentence))    # join all words with spaces and show the result
```

**Output:**

```text
After 'I' the model picks 'love' with probability 0.6
After 'love' the model picks 'chai' with probability 0.7
After 'chai' the model picks 'every' with probability 0.55
After 'every' the model picks 'morning' with probability 0.8
After 'morning' the model picks '<end>' with probability 1.0
Final sentence: I love chai every morning
```

**How the code works:**

- `next_word_table` is a dictionary that stores, for each word, the possible next words and their probabilities.
- `sentence` collects the words chosen so far, and `current_word` remembers the latest one.
- The `while` loop repeats the prediction step until the stop marker `<end>` is chosen.
- `max(options, key=options.get)` selects the word with the highest probability from the current options.
- Each chosen word is added to the sentence, so the output is built one word at a time, exactly like the five steps above.

**How a real LLM differs from this program:**

- A real model has a vocabulary of tens of thousands of tokens, not a handful of words.
- It does not use a lookup table. A neural network calculates the probabilities by looking at **all the text so far**, not only the last word.
- A **neural network** is a layered mathematical system whose parameters were set during training. In simple words, it is the machinery that turns the text so far into the probabilities.

### Activity 2: Complete the Phrase

Complete each phrase below with the first word that comes to your mind. Then write down a **second** word that could also fit.

1. "Jai ___"
2. "Happy Birthday to ___"
3. "Roses are red, violets are ___"
4. "Atithi Devo ___"

For each phrase, notice that one word feels far more likely than the others, and a few other words are still possible. A language model works with the same idea, except it gives a probability to every possible word instead of picking one in your head.

---

## Tokens and Tokenizers

In the section above, the model worked with "pieces" of text. This section explains what those pieces really are and how they are created.

### Token

- **Official Definition:** A token is the smallest unit of text that a language model reads and produces, which can be a whole word, part of a word, a number, or a punctuation mark.
- **In Simple Words:** A token is a small bite of text that the model can handle.
- **Real-Life Example:** Cutting a roti into bite-sized pieces before eating it. The model also "eats" text in bite-sized pieces.

### Tokenizer

- **Official Definition:** A tokenizer is the component that splits text into tokens and converts each token into a number called a token ID.
- **In Simple Words:** It is the "cutter" that prepares your text before the model reads it.
- **Real-Life Example:** Breaking a long word into syllables when you are learning to read.
- The model **cannot read letters directly**. It works only with numbers, so every token is mapped to a token ID.
- Different models often use **different tokenizers**, each with a fixed vocabulary, so the same sentence can give different token counts in different models.

### Subword Tokenization

- **Official Definition:** Subword tokenization is a method that splits text into pieces that are often smaller than a whole word, using a fixed list of common pieces.
- **In Simple Words:** Common words stay in one piece, and rare or long words are broken into smaller known pieces.
- **Real-Life Example:** A word such as "unhappiness" may be split into "un", "happi", and "ness", the way you can read an unfamiliar word by recognising its parts. This split is only an illustration, and a real tokenizer may split it differently.
- Subword splitting is **needed** because a tokenizer with a separate token for every possible word would become far too large.
- It also lets the model handle **new or rare words**, because they can be built from known pieces.

### Useful Rules of Thumb

- In English text, one token is roughly **four characters**, or about **three-quarters of a word**.
- So 100 tokens is about 75 words in English.
- Text in many other languages, including Hindi, often needs **more tokens for the same meaning**, because many tokenizers were built mostly from English text. The exact numbers depend on the tokenizer.

### A Toy Tokenizer in Code

The program below splits words using a small list of known pieces. At each position, it picks the **longest** known piece that fits, and it falls back to a single character when nothing fits.

```python
# A small vocabulary of known pieces (a real tokenizer has tens of thousands of pieces)
vocabulary = ["un", "happi", "ness", "play", "ing", "chai", "s"]  # the pieces our toy tokenizer knows

def tokenize(word):                                       # define a function that splits one word into pieces
    tokens = []                                           # this list will collect the pieces we find
    position = 0                                          # start reading from the first character
    while position < len(word):                           # keep going until we have covered the whole word
        match = None                                      # no piece has matched yet at this position
        for piece in vocabulary:                          # try every known piece one by one
            if word.startswith(piece, position):          # check if the word has this piece at the current position
                if match is None or len(piece) > len(match):  # keep the longest matching piece
                    match = piece                         # remember this piece as the best match so far
        if match is None:                                 # if no known piece fits here
            match = word[position]                        # fall back to a single character
        tokens.append(match)                              # save the piece we chose
        position += len(match)                            # move forward by the length of that piece
    return tokens                                         # give back the list of pieces

for word in ["unhappiness", "playing", "chais", "xyz"]:   # test the function on four words
    pieces = tokenize(word)                               # split the word into pieces
    print(word, "->", pieces, "->", len(pieces), "tokens")  # show the pieces and how many there are
```

**Output:**

```text
unhappiness -> ['un', 'happi', 'ness'] -> 3 tokens
playing -> ['play', 'ing'] -> 2 tokens
chais -> ['chai', 's'] -> 2 tokens
xyz -> ['x', 'y', 'z'] -> 3 tokens
```

**How the code works:**

- `vocabulary` holds the known pieces, and the function `tokenize` splits one word using them.
- `position` tracks how far into the word the program has read.
- At each position, the loop checks every known piece and keeps the **longest** one that matches.
- If no piece matches, as with "xyz", the program falls back to single characters, so every word can still be split.
- Words made of known pieces need few tokens, while unfamiliar words need more tokens.

### Activity 3: Count Tokens

Open any free online token counter for language models. Paste the two sentences below one at a time and write down the token count for each.

- "I drink chai every morning."
- "मैं हर सुबह चाय पीता हूँ।"

Both sentences carry the same meaning. Compare the two counts and note which one needed more tokens.

Then paste a long word of your choice, such as "internationalisation", and see how many tokens it needs. The exact numbers differ from tool to tool, so focus on the pattern.

---

## The Context Window

Every token you send and every token the model produces has to be held somewhere while the model works. That "somewhere" has a fixed size, which is called the context window.

### Context Window

- **Official Definition:** The context window is the maximum number of tokens a model can take into account at one time, including both the input it receives and the output it generates.
- **In Simple Words:** It is the model's working space. Anything outside it is not visible to the model.
- **Real-Life Example:** Your study desk has space for only a few open books. To place a new book on the desk, you must remove one that is already there.
- The window is measured in **tokens**, not in words or pages.
- Context window sizes differ from model to model, so check the limit of the tool you are using.

### What Counts Against the Context Window

- The **hidden instructions** that the tool gives the model before your message, if any.
- The **earlier messages** in the same chat, because they are sent again with each new message.
- Your **current prompt**, including any text you paste or attach.
- The **model's own reply**, because every generated token also takes space.
- All of these share one budget. A long prompt leaves less room for the reply.

### Common Doubts

- **Does the model remember me from one chat to another?** No. It sees only what is inside the current context window, so a new chat starts with an empty window.
- **What happens when the window is full?** Depending on the tool, older messages may be dropped, the request may be rejected, or the reply may be cut short.
- **Is a bigger window always better?** Not always. More tokens usually mean more cost and more time, and a long, unfocused input can make it harder for the model to use the important parts.

### Activity 4: Work Out the Budget

A model has a context window of **4,000 tokens**. Use the numbers below to answer the questions on paper.

| Item | Tokens |
|---|---|
| Hidden instructions from the tool | 300 |
| Earlier messages in the chat | 2,200 |
| Your new prompt | 500 |

1. How many tokens are left for the model's reply?
2. Using the rule of thumb that 100 tokens is about 75 English words, roughly how many words is that?
3. The reply you need is about 1,400 tokens long. What goes wrong, and what could you do about it?

**Check your answers:**

- 1: 4,000 − 300 − 2,200 − 500 = **1,000 tokens**.
- 2: 1,000 tokens is about **750 words**.
- 3: The reply does not fit, so it may be cut short or the tool may drop older messages. You can start a new chat, shorten the earlier history, or shorten your prompt to free up space.

---

## Temperature

You have seen that the model selects one token from a set of probabilities. A setting called temperature decides how that selection is made.

### Temperature

- **Official Definition:** Temperature is a setting that controls how much randomness a language model uses when it selects the next token.
- **In Simple Words:** It is a dial between "play safe" and "try something different".
- **Real-Life Example:** Making chai by following the exact recipe gives the same taste every time, while improvising with extra masala gives a different result each time.
- **Low temperature** makes the model choose the most likely tokens almost every time, so outputs are steady and repeatable.
- **High temperature** gives less likely tokens a better chance, so outputs become more varied, and they can also become odd or less accurate.

### Effect of Raising the Temperature

| Setting | What happens | Better suited for |
|---|---|---|
| Low (close to 0) | Most likely token is chosen almost always, so answers are steady | Factual answers, data extraction, code |
| Medium | A balance between steady and varied | Everyday writing and explanations |
| High | Less likely tokens are chosen more often, so answers vary a lot | Brainstorming names, slogans, creative writing |

- The **range and default value** of temperature differ across tools, so read the tool's documentation.
- Temperature changes **how a token is chosen**, not what the model knows, so a higher temperature does not make the model smarter.
- Even at the lowest setting, outputs are **usually but not always identical** from one run to the next.

### Temperature in Code

The program below takes the probabilities of the next word after "I drink a cup of". It reshapes them for three temperatures and picks a word 1,000 times for each, so you can see how the variety changes.

```python
import random                                             # import the random module to make random choices

options = {"chai": 0.60, "coffee": 0.25, "water": 0.10, "juice": 0.05}  # next-word probabilities after "I drink a cup of"

def apply_temperature(probabilities, temperature):        # define a function that reshapes probabilities using temperature
    adjusted = {}                                         # this dictionary will hold the reshaped values
    for word, p in probabilities.items():                 # go through every word and its probability
        adjusted[word] = p ** (1 / temperature)           # low temperature boosts the top word, high temperature evens things out
    total = sum(adjusted.values())                        # add up all the reshaped values
    for word in adjusted:                                 # go through every word again
        adjusted[word] = adjusted[word] / total           # divide by the total so the values add up to 1 again
    return adjusted                                       # give back the new probabilities

random.seed(7)                                            # fix the random starting point so the output is repeatable
for temperature in [0.2, 1.0, 2.0]:                       # try a low, a medium and a high temperature
    adjusted = apply_temperature(options, temperature)    # reshape the probabilities for this temperature
    words = list(adjusted.keys())                         # make a list of the candidate words
    weights = list(adjusted.values())                     # make a list of their new probabilities
    picks = random.choices(words, weights=weights, k=1000)  # pick a word 1000 times using those probabilities
    print("Temperature", temperature)                     # show which temperature we are testing
    for word in words:                                    # go through each candidate word
        print("  ", word, ":", picks.count(word), "out of 1000")  # show how often the word was picked
```

**Output:**

```text
Temperature 0.2
   chai : 989 out of 1000
   coffee : 11 out of 1000
   water : 0 out of 1000
   juice : 0 out of 1000
Temperature 1.0
   chai : 581 out of 1000
   coffee : 268 out of 1000
   water : 108 out of 1000
   juice : 43 out of 1000
Temperature 2.0
   chai : 455 out of 1000
   coffee : 270 out of 1000
   water : 167 out of 1000
   juice : 108 out of 1000
```

**How the code works:**

- `options` stores the original probabilities, where "chai" is the most likely word.
- `apply_temperature` raises each probability to the power of `1 / temperature`, and then divides by the total so that everything adds up to 1 again.
- At a **low temperature** (0.2), the top word becomes overwhelmingly likely, so "chai" is picked almost every time.
- At a **high temperature** (2.0), the probabilities become more even, so "coffee", "water", and "juice" are picked far more often.
- `random.choices` makes 1,000 weighted picks, and `picks.count(word)` shows how many times each word was chosen. Your counts may differ slightly on another setup, but the pattern stays the same.
- Real models apply temperature to their internal scores before converting them to probabilities, and the effect on variety is the same as in this simplified program.

### Activity 5: Predict, Then Test

Run the temperature program in Google Colab or any Python environment, and then follow these steps.

1. Change the list `[0.2, 1.0, 2.0]` to `[0.5, 1.0, 5.0]`.
2. **Before running**, write down your prediction: at temperature 5.0, will "chai" be picked more often or less often than at temperature 2.0?
3. Run the program and compare the counts with your prediction.
4. Write one sentence that describes what happens to the variety of outputs as the temperature rises.

---

## Why Fluent Text Can Be Factually Wrong

The model's output often reads smoothly and sounds sure of itself. This section explains why smooth language is not proof of correct facts.

### Hallucination

- **Official Definition:** A hallucination is an output from a language model that is fluent and confident but false, made up, or not supported by any real source.
- **In Simple Words:** The model says something that sounds right but is not true.
- **Real-Life Example:** A friend who has never visited a town still gives you detailed directions instead of admitting he does not know the way.
- Hallucinations happen because the model is trained to produce **likely next tokens**, not to check whether a statement is true.
- A sentence can be perfectly fluent and still false, because fluent language and correct facts are **two different things**.

### Why It Happens

- **Training goal:** The model learns which text usually follows which text, and there is no built-in fact-checking step.
- **Gaps in knowledge:** If the training data had little or no information about a topic, the model may still produce a confident-sounding answer built from similar patterns.
- **Fixed knowledge:** The model learned from data collected up to a certain date, so it may not know recent events unless the tool gives it access to current information.
- **Confident tone:** The model writes in the same smooth style whether it is right or wrong, so the tone gives you no warning.

### Common Doubts

- **Is a hallucination a bug that will be fixed completely?** It is a natural side effect of how next-token prediction works, so it can be **reduced but not removed entirely**.
- **Can I trust a reply if it includes names, numbers, or links?** Treat such details with extra care, because made-up details can look very convincing. Always verify them from a reliable source.

### Ways to Reduce the Risk

- **Supply the source text** inside your prompt and tell the model to answer only from it.
- **Add a constraint** such as "If you are not sure, say that you do not know."
- **Use a low temperature** for factual tasks, so the model sticks to the most likely tokens.
- **Verify important facts** from trusted sources before you use them.

### Activity 6: Test an AI Tool with an Invented Topic

Open any AI chat tool and follow these steps.

1. Type this prompt: "Summarise the plot of the novel *The Mango Clock of Meerut* by Aditi Varshney." This book title is invented for the activity.
2. Read the reply and note whether the tool says it does not know the book or whether it describes a plot with confidence.
3. Start a new chat and type the same prompt with this line added at the end: "If you do not know this book, say 'I do not know' and do not guess."
4. Compare the two replies and write down what the added constraint changed.

---

## Writing a Well-Formed Prompt

Everything so far points to one practical conclusion. The model completes whatever text it is given, so the quality of the output depends strongly on the quality of the input. That input is called the prompt.

### Prompt

- **Official Definition:** A prompt is the input text that you give to a language model to tell it what response you want.
- **In Simple Words:** A prompt is your instruction or question to the AI.
- **Real-Life Example:** Telling a tailor "stitch a shirt" gives a very different result from giving the measurements, the fabric, the style, and the deadline.
- A vague prompt leaves the model with **many possible directions**, so it picks one on its own.
- A clear prompt narrows those directions, so the model's probabilities favour the output you actually want.

A well-formed prompt contains four elements: **task**, **context**, **format**, and **constraints**.

### Task

- **Official Definition:** The task is the specific action that you want the model to perform.
- **In Simple Words:** It answers the question, "What do I want done?"
- **Real-Life Example:** Telling the tailor, "Stitch a shirt."
- Start with a **clear action word** such as write, summarise, explain, compare, or list.
- A prompt without a clear task is the **most common reason** for an unusable reply.

### Context

- **Official Definition:** Context is the background information that the model needs in order to do the task correctly.
- **In Simple Words:** It answers the question, "What should the model know before it starts?"
- **Real-Life Example:** Giving the tailor your measurements and telling him the shirt is for a wedding.
- Context can include **who you are, who the reader is, and the facts or text to work on**.
- **Common doubt:** The *context* in a prompt is the background information you supply, while the *context window* is the size limit that this information must fit into.

### Format

- **Official Definition:** Format is the structure or layout that you want the response to follow.
- **In Simple Words:** It answers the question, "What should the answer look like?"
- **Real-Life Example:** Telling the tailor you want a full-sleeve shirt with a collar.
- Examples of format are a **table**, a **numbered list**, a **three-paragraph email**, or a **short summary with a heading**.
- Without a format, the model chooses its own layout, and that layout may not suit your use.

### Constraints

- **Official Definition:** Constraints are the limits and rules that the response must follow, such as length, tone, or things to avoid.
- **In Simple Words:** They answer the question, "What are the boundaries?"
- **Real-Life Example:** Telling the tailor the shirt must be ready by Friday and must cost under ₹800.
- Common constraints are **word limits, tone, reading level, and topics to leave out**.
- Constraints also help reduce hallucination, for example "Use only the text provided" or "Say you do not know if unsure".

### The Four Elements Together

| Element | Question it answers | Example |
|---|---|---|
| **Task** | What should the model do? | Write a leave application email to my class coordinator. |
| **Context** | What should it know? | I am a second-year BCA student. I have had fever for three days and need leave from 12 to 14 March. |
| **Format** | What should the answer look like? | A subject line followed by three short paragraphs. |
| **Constraints** | What are the limits? | Under 120 words, polite tone, no medical details. |

**A weak prompt:**

```text
Write a leave letter.
```

**A well-formed prompt:**

```text
Write a leave application email to my class coordinator.

I am a second-year BCA student. I have had fever for three days
and need leave from 12 to 14 March.

Give a subject line followed by three short paragraphs.

Keep it under 120 words, use a polite tone, and do not include
medical details.
```

- The weak prompt leaves out who the letter is for, why leave is needed, and how it should look, so the model must guess all of it.
- The well-formed prompt contains all four elements, so there is much less to guess.
- A **longer prompt is not automatically better**. Include the details that matter and leave out the rest, because every extra token uses space in the context window.

### Activity 7: Rewrite the Prompt

Read this weak prompt: "Explain photosynthesis."

1. Write down which of the four elements are missing.
2. Rewrite the prompt so that it includes a clear task, context, format, and constraints. For example, decide who the explanation is for, how it should be laid out, and how long it should be.
3. Run your rewritten prompt in an AI chat tool and check whether the reply matches what you asked for in each element.
4. Remove one element, such as the format, and run it again to see how the reply changes.

---

## Putting Everything Together

The topics in this lecture are not separate facts. They describe one pipeline that runs every time you send a prompt.

1. You write a prompt with a **task**, **context**, **format**, and **constraints**.
2. The **tokenizer** splits the prompt into tokens, and these tokens count against the **context window** together with the earlier messages.
3. The model performs **next-token prediction** and calculates a probability for every possible next token.
4. The **temperature** setting controls how the next token is selected from those probabilities.
5. The selected token is added to the text, and the cycle repeats, with every new token also using space in the context window.
6. The result is fluent text, and because of **hallucination**, you verify important facts before relying on them.

---

## Key Takeaways

- **AI** is the widest term, **machine learning** is the part of AI that learns from data, and **generative AI** is the part of machine learning that creates new content, with LLMs focused on text.
- An LLM produces a response through **next-token prediction**: it calculates probabilities, selects one token, adds it to the text, and repeats.
- A **tokenizer** splits text into subword tokens, and the **context window** limits the total tokens of input and output together.
- A higher **temperature** makes outputs more varied, and fluent text can still be wrong, so important facts must be checked.
- A well-formed prompt has four elements: **task**, **context**, **format**, and **constraints**.

These ideas explain why small changes in a prompt can change the output so much. In the next session, you will build on this understanding to write and refine prompts more deliberately.

---

## Important Commands, Libraries, Terminologies used

| Term / Command | Meaning |
|---|---|
| **AI (Artificial Intelligence)** | Systems that perform tasks which normally need human intelligence |
| **Machine Learning (ML)** | AI that learns patterns from data instead of fixed rules |
| **Generative AI** | ML systems that create new content such as text, images, audio, or code |
| **LLM (Large Language Model)** | A generative AI model trained on very large amounts of text |
| **Model** | A trained program that applies learned patterns to new input |
| **Training** | Adjusting a model using large amounts of data |
| **Parameters** | Internal numbers set during training that store what the model learned |
| **Neural network** | Layered mathematical system that calculates the probabilities in a real LLM |
| **Token** | Smallest unit of text the model reads and produces |
| **Tokenizer** | Component that splits text into tokens and maps each to a token ID |
| **Subword tokenization** | Splitting text into common pieces that may be smaller than whole words |
| **Next-token prediction** | Calculating probabilities for the next token and selecting one, repeatedly |
| **Context window** | Maximum tokens a model can consider at once, including input and output |
| **Temperature** | Setting that controls randomness when the next token is selected |
| **Hallucination** | Fluent, confident output that is false or unsupported |
| **Prompt** | The input text that tells the model what response you want |
| **Task / Context / Format / Constraints** | The four elements of a well-formed prompt |
| `random` | Python module used to make random choices |
| `random.seed(n)` | Fixes the starting point of randomness so results can be repeated |
| `random.choices(list, weights=..., k=...)` | Picks `k` items from a list using the given weights |
| `max(dictionary, key=dictionary.get)` | Returns the dictionary key with the highest value |
| `word.startswith(piece, position)` | Checks whether a word has a given piece starting at a given position |
| `" ".join(list)` | Joins list items into one string with spaces between them |
