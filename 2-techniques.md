# Zero-shot Prompting

- Zero-shot prompting means that the prompt used to interact with the model won't contain examples or demonstrations.
- Instruction tuning has been shown to improve zero-shot learning. [Wei et al. 2022](https://arxiv.org/pdf/2109.01652)
- Instruction tuning is essentially the concept of finetuning models on datasets described via instructions.

# Few-shot Prompting

- Few-shot prompting can be used as a technique to enable in-context learning where we provide demonstrations in the prompt to steer the model to better performance.
- Tips([Min et al. 2022](https://arxiv.org/abs/2202.12837))
  - The label space and the distribution of the input text specified by the demonstrations are both important.
  - The format you use also plays a key role in performance, even if you just use random labels.
  - Random labels from a true distribution of labels (instead of a uniform distribution) also helps.
- A good case

  - Prompt

    ```text
    // Randomized, kept the format

    This is awesome! // Negative
    This is bad! // Positive
    Wow that movie was rad! // Positive
    What a horrible show! //
    ```

  - Output
    ```text
    Negative
    ```

- Limitations of Few-shot Prompting
  - It is still not a perfect technique, especially when dealing with more complex reasoning tasks.
  - It might help if we break the problem down into steps and demonstrate that to the model. -> chain-of-thought (CoT) prompting

# Chain-of-Thought(CoT) Prompting

Introduced in [Wei et al. (2022)](https://arxiv.org/abs/2201.11903), chain-of-thought (CoT) prompting enables complex reasoning capabilities through intermediate reasoning steps

![Chain-of-Thought Prompting](./imgs/2-techniques-1.png)

## 1. Zero-shot CoT Prompting

- [Kojima et al. (2022)](https://arxiv.org/abs/2205.11916) showed that just adding "Let's think step by step" to the original prompt can improve the reasoning capabilities of the model.
- An example
  - Prompt
    ```text
    I went to the market and bought 10 apples.
    I gave 2 apples to the neighbor and 2 to the repairman.
    I then went and bought 5 more apples and ate 1.
    How many apples did I remain with?
    Let's think step by step.
    ```
  - Output
    ```text
    First, you started with 10 apples.
    You gave away 2 apples to the neighbor and 2 to the repairman, so you had 6 apples left.
    Then you bought 5 more apples, so now you had 11 apples.
    Finally, you ate 1 apple, so you would remain with 10 apples.
    ```

## 2. Automatic Chain-of-Thought (Auto-CoT)

- [Zhang et al. (2022)](https://arxiv.org/abs/2210.03493) propose an approach to leverage LLMs with "Let's think step by step" prompt to generate reasoning chains.
- To mitigate effects of mistakes in the generated chains, the diversity of demonstrations matters.

![Auto-CoT](./imgs/2-techniques-2.png)

1. Question clustering: partition questions of a given dataset into a few clusters
2. Demonstration sampling: select a representative question from each cluster and generate its reasoning chain using Zero-Shot-CoT with simple heuristics

# Self-Consistency

- Proposed by [Wang et al. (2022)](https://arxiv.org/abs/2203.11171), self-consistency aims "to replace the naive greedy decoding used in chain-of-thought prompting".
- The idea is to sample multiple, diverse reasoning paths through few-shot CoT, and select the most consistent answer.

![Self-Consistency](./imgs/2-techniques-3.png)

1. A language model is prompted with a set of manually written chain-of-thought exemplars.
2. We sample a set of candidate outputs from the language model’s decoder, using sampling algorithms, including temperature sampling, top-k sampling, and nucleus sampling.
3. We aggregate the answers by marginalizing out the sampled reasoning paths and taking a majority vote directly over each answer to choose the most consistent answers.

- An example in the paper
  ![the-paper-snippet](./imgs/2-techniques-4.png)

# Generated Knowledge Prompting
