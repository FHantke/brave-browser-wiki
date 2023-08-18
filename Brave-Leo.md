# Leo

Brave Leo is a completely self-hosted chat assistant, available to Brave users for unparalleled privacy and performance. The underpinnings of Leo are based on Llama 2, [a foundation model introduced by Meta](https://about.fb.com/news/2023/07/llama-2/) with a special focus on safety. We make sure that user inputs are always submitted anonymously through a reverse-proxy to our serving infrastructure.

We specifically tuned the model prompt to adhere to Brave's core values, while still “answering” the requested task. Additionally, we have put extra safeguards in place to protect the user from being presented with toxic output. However, as with any other LLM, the outputs of the model should be treated with care for potential inaccuracies or errors.

## Versions

* 0.1 - Llama 2.0, no additional fine-tuning