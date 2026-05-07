# ABCDigits

ABCDigits is a synthetic key-value retrieval test for evaluating long-context retrieval ability in a controlled, semantics-free setting.

This repository provides text-generation utilities for constructing ABCDigits examples as described in our paper, *Screening Is Enough*:
https://arxiv.org/abs/2604.01178

## Overview

Each example consists of a sequence of uppercase letter-to-digit mappings such as:

```text
A=967892
W=900383
J=707723
W=900383
X=487610
[...]
P=933973
W=900383
L=169428
A=967892
V=608665
[...]
W=900383
P=933973
A=967892
L=169428
```

To construct the evaluation input, remove the last n_digits characters from the final line (i.e., the value of the queried key) and feed the resulting text to the model. The model is then expected to complete the missing digits.

## Usage

### Fixing the number of lines

```python
n_digits = 6
txts = generate_by_line_count(n_lines=2**6, depth=0.1, n_digits=n_digits, n_trials=1000)
prompt_txts = [t[:-n_digits] for t in txts]
answer_txts = [t[-n_digits:] for t in txts]
```

### Fixing the number of tokens (approximately)

```python
n_digits = 6
tokenizer = GPT2TokenizerFast.from_pretrained('gpt2')
txts = generate_by_token_count(n_tokens=400, depth=0.1, tokenizer=tokenizer, n_digits=n_digits, n_trials=1000)
prompt_txts = [t[:-n_digits] for t in txts]
answer_txts = [t[-n_digits:] for t in txts]
```

## Notes
- depth specifies the relative position of the target mapping in the context.
- n_digits controls the number of digits assigned to each key.
- n_trials is the number of generated examples.
- When using generate_by_token_count, the resulting token count may vary slightly depending on the tokenizer.

## Citation

If you use ABCDigits in your work, please cite our paper.

```text
@article{nakanishi2026screening,
  title={Screening Is Enough},
  author={Nakanishi, Ken M.},
  journal={arXiv preprint arXiv:2604.01178},
  year={2026}
}
```