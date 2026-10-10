# arXiv

Curated PDF library for the AI lab, kept at `white-papers/arXiv/`. Not a separate repo, and not part of the public site. Numbered filenames for stable ordering.

Three shelves:

1. **Deep learning foundations** — vision/sequence milestones that made modern NNs work  
2. **Citation canon (LLM history)** — Transformer → open foundation models  
3. **Systems / lab track (2022–26)** — vendor reports used for local finetune / product work  

## Index

### C. Deep learning foundations (NN history)

| # | File | Year | Why it matters |
|---|------|------|----------------|
| 23 | **LSTM** (Hochreiter & Schmidhuber) | 1997 | Long-range sequence memory |
| 24 | **Word2Vec** | 2013 | Dense word embeddings |
| 25 | **Seq2Seq** | 2014 | Encoder–decoder sequence models |
| 26 | **GANs** | 2014 | Generative adversarial training |
| 27 | **Adam** | 2014 | Default deep learning optimizer |
| 22 | **AlexNet** | 2012 | Deep CNNs + ImageNet — DL boom |
| 28 | **ResNet** | 2015 | Residual networks — train very deep nets |
| 29 | **Batch Normalization** | 2015 | Stabilize / accelerate training |

### A. Citation canon (history of LLMs)

| # | File | Year | Why it matters |
|---|------|------|----------------|
| 01 | Attention Is All You Need (Transformer) | 2017 | Attention architecture — foundation of modern LLMs |
| 12 | BERT | 2018 | Pretrained bidirectional encoders |
| 13 | GPT-2 | 2019 | Unsupervised multitask LM |
| 03 | GPT-3 few-shot | 2020 | In-context learning at scale |
| 14 | T5 | 2020 | Text-to-text unified framework |
| 15 | Scaling laws (Kaplan et al.) | 2020 | Loss vs compute/data/params |
| 16 | RAG | 2020 | Retrieval-augmented generation |
| 17 | LoRA | 2021 | Parameter-efficient finetuning |
| 02 | Chinchilla | 2022 | Compute-optimal training |
| 18 | InstructGPT | 2022 | RLHF / instruction following |
| 19 | Chain-of-Thought | 2022 | Multi-step reasoning |
| 04 | Constitutional AI | 2022 | Alignment techniques |
| 20 | FlashAttention | 2022 | Exact, memory-efficient attention |
| 21 | LLaMA | 2023 | Open efficient foundation models |

### B. Systems / lab track (architecture & products)

| # | File | Year | Lab use |
|---|------|------|---------|
| 05 | DeepSeek-V2 MLA/MoE | 2024 | Modern MoE architecture |
| 06 | Qwen2.5 tech report | 2024 | **Ship track** (qwen-finetune → Nakshatra) |
| 07 | Kimi linear attention | 2025 | Long-context / linear attention |
| 08 | GLM-4 / ChatGLM | 2024 | Chinese + bilingual systems |
| 09 | Gemini multimodal | 2023 | Multimodal stack reference |
| 10 | Grok-4 model card | 2025 | xAI product reference |
| 11 | AgentWorld / language world models | 2026 | Agent / world-model research |

### Lab tags (quick)

| Tag | Papers |
|-----|--------|
| **qwen ship track** | 06, 17 |
| **ornith / local research** | 17, 21, 05 |
| **DL foundations** | 22–29 |
| **LLM foundations** | 01, 12–21, 02–04 |
| **systems reference** | 05–11 |

## Sources

Open-access PDFs: arXiv, NeurIPS proceedings, OpenAI technical reports, archived LSTM reprint.

## Optional backlog

See `NEXT.md` (Dropout, VGG, ViT, AlphaGo, etc.).

## Count

**29** PDFs total (01–29).
