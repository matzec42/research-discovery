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



### Challenge 4 - 

- 