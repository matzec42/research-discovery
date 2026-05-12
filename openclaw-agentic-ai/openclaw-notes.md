# OpenClaw Repo Notes

## From the live workshop w/ Will (5/7/2026)

### Challenge 1 - Curl Basics --- Reflection Qs

1. API requires an `Authorization` header to regulate access and use. Like any API, it takes resources and there is cost/expense to using it. Authorization allows the user to use (and the API to manage/expend) resources according to their level of privilege (e.g., paid users, free users). Possible constraints for security reasons as well---perhaps certain API keys sent in the header correspond with security policies for the API (granat least amount of access needed, user vs admin, etc.).

2. `messages` field as an array suggests memory---as conversations continue, model consumes them for context/improved responses with more interactions. Logging for tracing/history as well (?)

3. `choices` as an array---thinking easier/more consistent access to and handling of data (arrays/objects are better than straight strings)



### Challenge 2 - Pipe and Execute

- Some nice review / new commands for CLI
- Piping ( `|` ) to create a chain / series of executed tasks:
    - curl to API --> process the response (jq, -r '.choices[0].message.content') --> run `bash` command
- Dangerous and never recommended to run a response without showing it to user, sandboxing the execution of agent (e.g., container), validating commands (in production level systems, things like human-in-the-loop, e.g.) before they are run



### Challenge 3 - JS Agent

- Writing it as JS code (Bun is run-time environment)
- Flow:
    - `POST` request to the API (native JS `fetch`) --> parse response --> print command (PoC) and run `bash` command (` await Bun.$`sh -c ${cmd}` `)
- Better / paid models will yield more consistent results, as would prompt refining...but with the free model, it does (mostly) take screenshots
    - variability with which screen (multi-monitor set up), file name, where it saves it (e.g., Desktop, in the project file directory, or elsewhere)



### Challenge 4 - While loop Agent

- **Memory** -- instantiate an array to hold a system prompt, and populate it with user prompts and responses from the AI
    - History acts as context and improves subsequent interactions
- `while` loop creates the **iterative** characteristic of AI
    - w/ the memory (saving successes, error messages, etc.), it can consume this and produce better responses
    - the `break` --- exits upon success
    - the `catch` block creates the **retrying** behavior
- The **feedback** loop when errors occur also is a characteristic of core LLM architecture



### Challenge 5 - Tool Calling

- Define a `BASH_TOOL` --- before, the agent itself was the command. Now, the model is acting like an orchestrator/planner
    - Tells the model that it can invoke functions (this variable uses plain language to define a bash command function)
    - It's included on the POST request ---> changes the behavior and capabilities of the API
- Still utilizing history as **memory** and to improve responses (**feedback loop** with the `while` loop)
- Model still iterates / retries
    - But now, it's a **"reasoning"** loop --- not just retrying, but:
        - Thinks --> use tool --> observe result --> think again --> use another tool --> observe result --> eventually answer user
        - i.e., closer to a real agent
- Tool messages get pushed into the history/memory as well
    - feeds shell output back to the model as structured tool output
    - tools are how the AI "sees" execution results
    - converation history starts with system, then goes something like this:
        - user --> assistant (tool call) --> tool (result) --> assistant (tool call) --> tool (result) --> assistant (final answer)
    - the model can also distinguish between user/human, assistant reasoning, and tool outputs (separation of data)

- NOTE: changed out model from `openrouter/free` due to too many errors/agent crashes. Likely because free models aren't as sophisticated and can't handle tooling. You can search for free models that do so on OpenRouter (e.g., using`openai/gpt-oss-20b:free` improved it considerably).