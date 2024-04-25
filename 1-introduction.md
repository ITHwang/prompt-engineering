# LLM Settings
1. Temperature
2. Top P(Nucleus Sampling)
    - Only the tokens comprising the `top_p` probability mass are considered for responses.
    - The lower the `top_p`, the more confident.
    - The higher the `top_p`, the more diverse.
    - Don't use Temperature and Top P together.
3. Max Length: The maximum number of tokens that the model can generate.
4. Stop Sequences: The sequence of tokens that the model should stop generating at.
5. Frequency Penalty
    - A penalty on the next token proportional to how many times that token already appears in the output so far.
    - The higher the penalty, the less repetitive the output.
6. Presence Penalty
    - A penalty on repeated tokens but, unlike the frequency penalty, the penalty is the same for all repeated tokens.
    - It prevents the model from repeating phrases too often in its response.
    - To be more creative, set a higher presence penalty.
    - To stay focused, set a lower presence penalty.
    - Don't use Frequency and Presence Penalty together.

# Basics of Prompting
- Roles: system, user, and assistant.
- Prompt Formatting
    - zero-shot prompting
        ```text
        Q: <Question>?
        A:
        ```
    - few-shot prompting(enable in-context learning)
        ```text
        Q: <Question>?
        A: <Answer>
        Q: <Question>?
        A: <Answer>
        Q: <Question>?
        A: <Answer>
        Q: <Question>?
        A:
        ```

# Elements of a Prompt 
- Instruction(a specific task)
- Context(external information)
- Input Data(question)
- Output Indicator(the type or format of the response)

# General Tips for Designing Prompts
1. Start Simple
    - Start with a simple prompt and keep adding specificity
    - Break down a big task into simpler sub-tasks
2. The Instruction
    - Just try a lot so that you can find the best commands, keywords, contexts, etc.
    - Recommand: Use the instruction as the first sentence of the prompt.
3. Specificity
    - The more descriptive and detailed the prompt is, the better the results.
    - But unnecessary details are not good in terms of maximum length of a prompt.
4. Avoid Impreciseness
    - the more direct and specific, the more effective the messaege get across.
    - A bad case
        ```text
        Explain the concept prompt engineering. Keep the explanation short, only a few sentences, and don't be too descriptive.
        ```
    - A Good case
        ```text
        Use 2-3 sentences to explain the concept of prompt engineering to a high school student.
        ```
5. Avoid saying what not to do but say what to do instead(e.g. "Don't think about ELEPHANTS")
    - A bad case
        - Prompt
            ```text
            The following is an agent that recommends movies to a customer. DO NOT ASK FOR INTERESTS. DO NOT ASK FOR PERSONAL INFORMATION.

            Customer: Please recommend a movie based on my interests.
            Agent: 
            ```
        - Output
            ```text
            Sure, I can recommend a movie based on your interests.
            What kind of movie would you like to watch? Do you prefer action, comedy, romance, or something else?
            ```
    - A Good case
        - Prompt
            ```text
            The following is an agent that recommends movies to a customer. The agent is responsible to recommend a movie from the top global trending movies. It should refrain from asking users for their preferences and avoid asking for personal information. If the agent doesn't have a movie to recommend, it should respond "Sorry, couldn't find a movie to recommend today.".

            Customer: Please recommend a movie based on my interests.
            Agent:
            ```
        - Output
            ```text
            Sorry, I don't have any information about your interests.
            However, here's a list of the top global trending movies right now: [list of movies]. I hope you find something you like!
            ```
6. Separate the Instruction and the Context
    - Prompt
        ```text
        Summarize the text below as a bullet point list of the most important points.

        Text: """
        {text input here}
        """
        ```
7. In Code Generation, use “leading words” or a code snippet to nudge the model toward a particular pattern.
    - Prompt(starting with "import")
        ```text
        # Write a simple python function that
        # 1. Ask me for a number in mile
        # 2. It converts miles to kilometers
        
        import
        ```

# References
- [Best practices for prompt engineering with OpenAI API](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)