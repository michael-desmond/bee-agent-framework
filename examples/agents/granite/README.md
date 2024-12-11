# 🐝 IBM Granite Bee

The [IBM Granite](https://www.ibm.com/granite) family of models can be used as the underlying LLM within Bee Agents. Granite™ is IBM's family of open, performant, and trusted AI models tailored for business and optimized to scale your AI applications.

This guide and the associated examples will help you get started with creating Bee Agents using Granite.

## 📦 Prerequisites

### LLM Services

IBM Granite is supported by [watsonx.ai](https://www.ibm.com/products/watsonx-ai) and [Ollama](https://ollama.com/). Watsonx.ai will allow you to run models in the cloud. Ollama will allow you to download and run models locally.

> [!TIP]
> Better performance will be achieved by using larger Granite models.

> [!NOTE]
> If you work for IBM there are additional options to run IBM Granite models with VLLM or RITS.

#### Ollama

There are guides available for running Granite with Ollama on [Linux](https://www.ibm.com/granite/docs/run/granite-on-linux/granite/), [Mac](https://www.ibm.com/granite/docs/run/granite-on-mac/granite/) or [Windows](https://www.ibm.com/granite/docs/run/granite-on-windows/granite/).

#### Watsonx

In order to use IBM [watsonx.ai](https://www.ibm.com/products/watsonx-ai) you can follow the [IBM watsonx as a Service](https://www.ibm.com/docs/en/watsonx/saas) documentation and in particular their [getting started tutorials](https://www.ibm.com/docs/en/watsonx/saas?topic=getting-started-tutorials).

## 🛠️ Getting Started

Before getting started, ensure you have access to an IBM Granite model and have followed any other requirements in the [prerequisites](#prerequisites) above.

The [granite_bee](/examples/agents/granite/granite_bee.ts) example agent is set up to demonstrate how to use IBM Granite with the Bee Agent Framework. In order to run this example, take the following steps:

1. Follow the [Local Installation](https://github.com/i-am-bee/bee-agent-framework?tab=readme-ov-file#local-installation) section of our main README to [Clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) the Bee Agent Framework git repository and install its dependencies. Return to this guide when you have completed these steps and are ready to configure environment variables.

1. Copy the [.env.template](/.env.template) file to a new file called `.env` and fill out the required details within the `.env` file (replace the values shown in braces), as follows

   1. For watsonx:

      ```.env
      LLM_BACKEND=watsonx
      WATSONX_API_KEY={YOUR_WATSONX_API_KEY}
      WATSONX_PROJECT_ID={YOUR_WATSONX_PROJECT_ID}
      WATSONX_REGION=us-south
      ```

      Note: [other regions](https://www.ibm.com/docs/en/watsonx/saas?topic=integrations-regional-availability-cloud) are also available

   1. For Ollama:

      ```.env
      LLM_BACKEND=ollama
      OLLAMA_HOST={http://0.0.0.0:11434}
      ```

1. Run the [granite_bee](/examples/agents/granite/granite_bee.ts) example:

   ```shell
   yarn run start examples/agents/granite_bee.ts
   ```

   This will show the various stages of the agent running and ultimately deliver an answer similar to the following:

   > Agent 🤖 : The current weather in London is 13.3°C with no rain. The maximum temperature for today is expected to be 13.5°C.

   The default prompt is the question `What is the current weather in London?`

   You can provide other prompts as follows:

   ```shell
    yarn run start examples/agents/granite/granite_bee.ts <<< 'Who is the president of the USA?'
    yarn run start examples/agents/granite/granite_bee.ts <<< 'What is the spanish word for dog?'
    yarn run start examples/agents/granite/granite_bee.ts <<< 'What does the alias command do in the BASH shell?'
    yarn run start examples/agents/granite/granite_bee.ts <<< "What’s the largest technology company by market capitalization right now?"
    yarn run start examples/agents/granite/granite_bee.ts <<< "What’s the weather going to be like tomorrow in Sydney?"
   ```

   The example is configured with 2 available [tools](/docs/tools.md), these are OpenMeteo for facilitating weather related prompts and DuckDuckGo for facilitating search related prompts. The above example prompts demonstrate how each of these tools can be exercised.

## 🤖 Granite wiki bee

The [granite_wiki_bee](/examples/agents/granite/granite_wiki_bee.ts) example agent is set up to demonstrate more advanced usage of the bee agent framework, specifically
performing information retrieval on tool output via the frameworks tool customization options. This agent is configured to use the `WikipediaTool` which enables the agent to
search wikipedia and access articles. However the tool interface is extended so that the agent can specify both a wikipedia topic and a search query within the topic. The wikipedia tool output is chunked and piped into a `SimilarityTool` that calculates the cosine similarity between the query and the data chunks. Only the most relevant chunks are then provided to the agents memory.

This example uses ollama exclusively.

To get started you will need to pull `granite3-dense:8b` and `nomic-embed-text` (to perform text embedding).

```shell
ollama pull granite3-dense:8b
ollama pull nomic-embed-text
```

Run the [granite_wiki_bee](/examples/agents/granite/granite_wiki_bee.ts) example:

```shell
yarn run start examples/agents/granite/granite_wiki_bee.ts
```

You will see the agent running and producing an answer similar to the following:

> Agent 🤖 : The authors of the paper 'Attention is all you need' are Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan Gomez, Lukasz Kaiser, and Illia Polosukhin. The paper has been cited more than 100,000 times as of 2024.

The default prompt is the question `Who were the authors of the paper 'Attention is all you need' and how many citations does it have?`

You can provide other prompts as follows:

```shell
   yarn run start examples/agents/granite/granite_wiki_bee.ts <<< 'When did the Roman Empire collapse?'
   yarn run start examples/agents/granite/granite_wiki_bee.ts <<< 'What is the Great Barrier Reef?'
   yarn run start examples/agents/granite/granite_wiki_bee.ts <<< 'Where is IBM headquartered?'
```

> [!TIP]
> Better performance will be achieved by using larger Granite models.