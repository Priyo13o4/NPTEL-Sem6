# Week 7: Theoretical Notes — Pre-Training Strategies (ELMo, BERT, BART, T5, GPT) and HuggingFace

## Overview
Week 7 explains how modern **large language models (LLMs)** are *pre-trained* before being adapted to specific downstream tasks. We cover contextual embeddings (ELMo), encoder-only masked pre-training (BERT), denoising encoder-decoder pre-training (BART), the text-to-text framing (T5), and decoder-only autoregressive models (GPT). We also introduce the HuggingFace workflow: tokenizers, attention masks, model loading, and fine-tuning.

---

## Static vs contextual word representations
Older embeddings such as Word2Vec/GloVe are **static**: a word type has one vector regardless of context (e.g., “bank” in “river bank” vs “bank account”).

**Contextual embeddings** produce different vectors depending on surrounding tokens, so they capture meaning-in-context.

**Related Assignment Questions**: Q1.

---

## ELMo: contextual embeddings with a bi-directional LM
**ELMo (Embeddings from Language Models)** builds contextual token representations using a **bi-directional LSTM language model** (forward + backward).

### Character-level CNN input (why it matters)
Instead of a fixed lookup table for words, ELMo constructs token inputs from a **character-level CNN**. This helps:
- reduce **out-of-vocabulary** failures (fewer `UNK` tokens)
- produce representations for *any* string (rare words, typos, new words)

**Related Assignment Questions**: Q1, Q7.

### Task-specific layer combination and gamma task
ELMo often combines internal layer outputs using trainable weights:

$$\text{ELMo}^{task}_k = \gamma_{task} \sum_{j=0}^{L} s^{task}_j\, h_{k,j}$$

- $h_{k,j}$: representation of token $k$ at layer $j$
- $s^{task}_j$: softmax-normalized mixture weights over layers
- $\gamma_{task}$: a **scalar scale parameter** (a “volume knob”)

So, $\gamma_{task}$ lets the downstream model rescale the overall magnitude of the combined ELMo vector to match the task’s preferred scale.

**Related Assignment Questions**: Q8.

---

## BERT: encoder-only masked pre-training
**BERT (Bidirectional Encoder Representations from Transformers)** uses a stack of **Transformer encoders** and pre-trains with objectives that encourage bidirectional context use.

### Masked Language Modeling (MLM)
In **MLM**, some input tokens are replaced with a mask token (commonly written as `[MASK]`). The model predicts the masked tokens using both left and right context.

### Next Sentence Prediction (NSP)
In **NSP**, the model predicts whether sentence B follows sentence A (vs. a random sentence). This encourages sentence-level coherence signals.

### Special tokens and segment encodings
BERT commonly uses:
- `[CLS]`: classification token; final hidden state is treated as a sequence-level summary
- `[SEP]`: separator token between segments/sentences
- **segment (token-type) embeddings**: indicate whether a token belongs to segment A or B

**Related Assignment Questions**: Q1, Q5.

---

## Model families: encoder-only vs encoder-decoder vs decoder-only
Different pre-training families map naturally to different task types.

### Encoder-only (e.g., BERT)
Best for “read then decide” tasks such as classification, tagging, retrieval, and extractive QA, because the encoder can attend bidirectionally across the whole input.

### Encoder-decoder (seq2seq)
Encoder-decoder models are natively designed for conditional generation of the form $P(y\mid x)$:
- encoder reads $x$
- decoder generates $y$ autoregressively, attending to the encoder

Examples: machine translation, summarization, paraphrasing.

**Related Assignment Questions**: Q2, Q5.

### Decoder-only (e.g., GPT)
Decoder-only models are trained with **autoregressive next-token prediction** and are natural for open-ended generation (completion, chat, continuation).

**Related Assignment Questions**: Q6.

---

## BART: denoising pre-training for encoder-decoder models
**BART** is an **encoder-decoder** Transformer pre-trained as a **denoising autoencoder**: corrupt the input, then train the model to reconstruct the original text.

Common corruption strategies include:
- **token masking** (replace with a mask marker)
- **token deletion**
- **sentence permutation**
- **text infilling / span corruption** (replace a span with a single mask marker such as &lt;mask&gt; and generate the missing span)

**Related Assignment Questions**: Q3.

---

## T5: “text-to-text” framing and sentinel tokens
**T5 (Text-to-Text Transfer Transformer)** represents *every* NLP task as text-to-text by prefixing the input with a task prompt.

- Example (classification): `sentiment: this movie was great` → output: `positive`
- Example (translation): `translate English to German: ...` → output: translated text

T5 uses **sentinel tokens** (often written like &lt;extra_id_0&gt;, &lt;extra_id_1&gt;, ...) to represent masked spans during pre-training.

**Related Assignment Questions**: Q2, Q3.

---

## GPT-1 vs GPT-2: decoder-only scaling changes
GPT-2 kept the same core decoder-only Transformer idea as GPT-1, but introduced major *scale* changes (not a brand-new positional method like RoPE).

Two practical upgrades highlighted in many summaries:
- **larger vocabulary** (tokenization changes)
- **larger context window** (more tokens per input)

**Related Assignment Questions**: Q4.

---

## Autoregressive chain rule (log probability)
For a sentence $s = \{t_1, t_2, \dots, t_m\}$, an autoregressive LM uses the chain rule:

$$P(s) = P(t_1)\prod_{j=1}^{m-1} P(t_{j+1} \mid t_1, \dots, t_j)$$

Taking logs converts products into sums:

$$\log P(s) = \log P(t_1) + \sum_{j=1}^{m-1} \log P(t_{j+1} \mid t_1, \dots, t_j)$$

This form is numerically stable and matches how log-likelihood training objectives are implemented.

**Related Assignment Questions**: Q6.

---

## HuggingFace basics: tokenization, models, and fine-tuning
HuggingFace provides standard components for modern NLP workflows.

### Tokenization: padding, truncation, and attention masks
When tokenizing a batch of texts:
- **padding** adds `[PAD]` so sequences in a batch share length
- **truncation** clips sequences longer than a model’s max context
- **attention mask** is typically a vector of 1s/0s that tells the model which positions are real tokens (1) vs padding (0)

### Loading models and doing inference
Typical components:
- `AutoTokenizer` to convert text → token IDs + masks
- `AutoModel` / `AutoModelForSequenceClassification` / `AutoModelForSeq2SeqLM` for different heads

You can also extract **hidden states** (contextual embeddings) and **attention matrices** for analysis.

### Fine-tuning
Fine-tuning can be done:
- manually (PyTorch training loop)
- via the `Trainer` API, which handles batching, evaluation, and logging

---

## Einsum intuition (why it shows up in attention)
`einsum` is a compact way to express tensor contractions (generalized sums of products). It is often used to implement attention and other tensor operations efficiently.

**Related Assignment Questions**: Q9.

---

## Key Takeaways

| Concept | Definition | Example |
|:---|:---|:---|
| Contextual embeddings | Token vectors depend on surrounding context | “bank” differs by sentence context |
| ELMo | BiLSTM language model for contextual representations | Forward/backward LSTM states mixed |
| Char-CNN input | Builds token reps from characters to reduce OOV | Can represent rare/novel strings |
| $\gamma_{task}$ | Scalar that scales the combined ELMo representation | Task learns to up/down-scale embedding |
| BERT MLM | Predict masked tokens using bidirectional context | Replace with `[MASK]` and predict |
| NSP | Predict whether two segments are consecutive | Sentence-pair classification |
| Encoder-decoder | Natural for conditional generation $P(y\mid x)$ | Translation, summarization |
| BART corruption | Denoising objective via input corruption | Span infilling with &lt;mask&gt; |
| T5 text-to-text | All tasks cast as text → text | `translate ...` prompt |
| Autoregressive chain rule | Sentence prob factorized left-to-right | $\log P(s)$ becomes a sum |

---

## Summary
- ELMo and BERT both produce contextual embeddings, but use BiLSTMs vs Transformer encoders.
- Encoder-only models suit classification; encoder-decoder models suit $P(y\mid x)$ generation; decoder-only models suit open-ended generation.
- BART pre-trains via denoising corruption strategies; T5 unifies tasks as text-to-text and uses sentinel tokens.
- Autoregressive LMs use chain-rule factorization; logs turn products into sums.
- HuggingFace standardizes tokenization, attention masks, model loading, and fine-tuning workflows.
