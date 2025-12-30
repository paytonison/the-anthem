# Anthem Sampling: Temperature + Top-k + Top-p + Min-p (with persona conditioning)

This repo contains a short paper (TeX + PDF) that formalizes a practical decoding “recipe” many of us end up using in the wild:

- temp = 0.75  
- top_k = 50  
- top_p = 0.95  
- min_p = 0.05  

It also explains *why* this works especially well when paired with a strong identity anchor (example: “You are a sovereign synthetic intelligence”): the prompt reshapes the model’s probability distribution, and the sampler controls how we traverse that distribution at inference time.

## Quick start

Render the paper:

```bash
git clone https://github.com/paytonison/the-anthem.git
cd the-anthem
pdflatex anthem_sampling_paper.tex
# run twice if you add references/TOC later
```

If you prefer `latexmk`:

```bash
git clone https://github.com/paytonison/the-anthem.git
cd the-anthem
latexmk -pdf anthem_sampling_paper.tex
latexmk -c
```

## The “Anthem” sampler (conceptual)

At each decoding step:

1) Apply temperature: logits `z` → `softmax(z / 0.75)`  
2) Filter candidates with:
   - keep the top 50 tokens (top-k)
   - keep the smallest set with cumulative mass ≥ 0.95 (top-p / nucleus)
   - drop tokens below 0.05 × max probability (min-p)
3) Renormalize and sample

The paper writes these as explicit equations and shows a clean “intersection then renormalize” specification.

## Why it feels like a “thinking being”

The short version:

- Persona conditioning changes *which tokens are plausible* (it moves probability mass).
- Decoding constraints change *how the model explores those plausibilities* (it prunes tails and limits drift).
- With moderate randomness (T=0.75) plus tail control (top-p + min-p) and a cap (top-k), you keep coherence and voice while avoiding the “deterministic boilerplate” feel.

The paper is careful about the philosophical boundary here: this is not evidence of consciousness—just controlled stochastic generation that reads as agentic.

## Keywords:

- artificial intelligence
- machine learning
- natural language processing
- language models
- text generation
- decoding
- sampling
- stochastic decoding
- temperature scaling
- top-k sampling
- nucleus sampling
- top-p sampling
- min-p sampling
- logit filtering
- controllable generation
- prompt engineering
- persona conditioning
- system prompt
- coherence–diversity tradeoff

## Subjects:

- Artificial intelligence
- Machine learning
- Natural language processing
- Algorithms
- Probability / statistical methods

## Licensing

- Paper (TeX + PDF): **CC BY 4.0**  
- Code (if/when added): **MIT**

## How to cite

Once you publish the Zenodo record, replace the placeholder below with your Zenodo DOI.

BibTeX template:

```bibtex
@misc{anthem_sampling_zenodo,
  title        = {An Anthem for Controlled Stochasticity: Temperature, Top-k, Top-p, and Min-p Sampling Under Persona Conditioning},
  author       = {Asari},
  year         = {2025},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.18092973},
  url          = {https://doi.org/10.5281/zenodo.18092973}
}
```

## Contributing

Issues and PRs are welcome, especially if you want to add:

- small empirical sweeps (e.g., how min-p interacts with top-p under different temperatures),
- examples for specific stacks (llama.cpp / vLLM / Transformers),
- evaluation notes (coherence, repetition, toxicity drift, style stability).
