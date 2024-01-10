# gpt-main
# ChatGPT
[ChatGPT](https://chat.openai.com/) is a text-based AI assistant by OpenAI. This is an analysis of ChatGPT.

## Analysis
### Models available via chat.openai.com
| Title | Slug | Max tokens | Description |
| --- | --- | --- | --- |
| Plugins | text-davinci-002-plugins | 8195 | An experimental model that knows when and how to use plugins |
| Default (GPT-3.5) | text-davinci-002-render-sha | 4097 | Optimized for speed, currently available to Plus users |
| Legacy (GPT-3.5) | text-davinci-002-render-paid | 4097 | The previous ChatGPT Plus model |
| GPT-4 | gpt-4 | 4095 | Our most advanced model, available to Plus subscribers. GPT-4 excels at tasks that require advanced reasoning, complex instruction understanding, and more creativity. |

### Models available via platform.openai.com
| Model | Max tokens | Training data (pre-training) |
| --- | --- | --- |
| gpt-4-0314 | 8192 | Up to Sep 2021
| gpt-4-32k-0314 | 32768 | Up to Sep 2021
| gpt-3.5-turbo-0301 | 4096 | Up to Sep 2021

### Vocabulary
The ChatGPT models employ a distinct vocabulary compared to their predecessors. The models use a unique `cl100k_base` vocabulary with `100,000` tokens, where each token encodes an average of `3.7` characters in English. For a detailed analysis, see [vocab.ipynb](vocab.ipynb). 

Additionally, these models employ the [Chat Markup Language](https://github.com/openai/openai-python/blob/main/chatml.md).

### gpt-3.5-turbo
If we send the message `[{"role": "user", "content": "13+37="}]` to the model, we get the following chat completion response:

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "50",
        "role": "assistant"
      }
    }
  ],
  "created": 123,
  "id": "chatcmpl-XXX",
  "model": "gpt-3.5-turbo-0301",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 1,
    "prompt_tokens": 12,
    "total_tokens": 13
  }
}
```

The number of prompt tokens and completion tokens are computed as follows:
```python
prompt_tokens = ['<|im_start|>', 'user', '\n', '13', '+', '37', '=', '<|im_end|>', '\n', '<|im_start|>', 'assistant',  '<|message|>']
# len(tokens) is 12
```

```python
completion_tokens = ['50']
# len(tokens) is 1
```

## gpt-4
The `gpt-4` model returns a different number of prompt tokens compared to `gpt-3.5-turbo`. If we send the message `[{"role": "user", "content": "13+37="}]` to the model, it returns the following chat completion response:
```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "50",
        "role": "assistant"
      }
    }
  ],
  "created": 123,
  "id": "chatcmpl-XXX",
  "model": "gpt-4-0314",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 1,
    "prompt_tokens": 11,
    "total_tokens": 12
  }
}
```

> It's unclear why the model returns 11 prompt tokens instead of 12. Maybe there is no `\n` after the role `user`.

## Performance
We have evaluated the ChatGPT model `gpt-4-0314` with the [HumanEval](https://github.com/openai/human-eval) dataset. Out of 164 programming problems, the model can solve `78.66%`.

| Model name | Pass@1 | Date | Comment | Completions of evaluation run | Prompt
| - | - | - | - | - | - |
| gpt-4-0314 | 78.66% | 2023-03-17 | https://openai.com/api/ | [2023-03-17-samples-gpt-4-0314.jsonl](2023-03-17-samples-gpt-4-0314.jsonl) | Complete the following code:\n{code}
| gpt-3.5-turbo-0301 | 72.56% | 2023-03-01 | https://openai.com/api/ | [2023-03-01-samples-gpt-3.5-turbo-0301.jsonl](2023-03-01-samples-gpt-3.5-turbo-0301.jsonl)
| text-davinci-002-render-sha | 70.12% | 2023-02-19 | https://chat.openai.com/ |  [2023-02-19-samples-text-davinci-002-render-sha.jsonl](2023-02-19-samples-text-davinci-002-render-sha.jsonl)
| text-davinci-002-render | 56.10% | 2022-12-03 | https://chat.openai.com/ |
| cushman-ml | 56.10% | 2022-10-23 | Copilot
| code-davinci-002 | 46.95% | 2022-10-23 | https://openai.com/api/
| code-cushman-001 | 32.93% | 2022-10-23 | https://openai.com/api/
