# What is Brave Leo?

Brave Leo is a chat assistant hosted by Brave without the use of third-party AI services, available to Brave users on the desktop Nightly channel. The model behind Leo is Llama 2, [a source-available large language model released by Meta](https://about.fb.com/news/2023/07/llama-2/) with a special focus on safety. We’ve made sure that user inputs are always submitted anonymously through a [reverse-proxy](https://en.wikipedia.org/wiki/Reverse_proxy) to our inference infrastructure. In this way, Brave can offer an AI experience with unparalleled privacy.

We’ve specifically tuned the model prompt to adhere to Brave's core values. However, as with any other LLM, the outputs of the model should be treated with care for potential inaccuracies or errors.
How to try Leo and share feedback
Leo is available today for all users of the Brave browser desktop Nightly channel. Nightly desktop users can access Leo via the  button in Brave Sidebar.

Are you a Brave Nightly user? Please tell us what you think of Leo! Just click the [link](https://community.brave.com/c/new-feature-feedback/141) to share feedback.

## A note on anonymity
Leo is free to use for any desktop Nightly user, and no user login or account is required. Chats in Leo cannot be used for training purposes, and no one can review those conversations, as they're not persisted on Brave’s servers—conversations are discarded immediately after the reply is generated. For this reason, there's no way to review past conversations or delete that data—it isn't stored in the first place.

## What data does the Brave browser send?
If you use Leo, the browser shares with the server your latest query, your ongoing conversation history and, when the use case calls for it, only the necessary context from the page you're actively viewing (e.g. the article’s text, or the youtube video transcript).

## How can I get better results out of Leo?
As with any AI, the more specific you are with your prompts and context, the better the results Leo can provide. Remember to give Leo clear, detailed instructions and, if you don't get exactly the answer you're looking for, to try wording your query/prompt a different way.

## Does Leo have access to live information?
For now, Leo does not have access to live info. However, in future releases we do plan to offer a version of Leo with some level of access to current information. This will be powered by our very own [Brave Search](https://brave.com/search/).

## What’s next for Brave Leo?
In addition to incorporating live info, we’ll be making improvements to Leo’s accuracy and user experience. We hope to release Leo to all Brave browser users in the coming months.

# Versions

* 0.1 - Llama 2.0, no additional fine-tuning