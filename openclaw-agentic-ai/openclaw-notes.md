# OpenClaw Repo Notes

## From the live workshop w/ Will (5/7/2026)

### Challenge 1 - Curl Basics --- Reflection Qs

1. API requires an `Authorization` header to regulate access and use. Like any API, it takes resources and there is cost/expense to using it. Authorization allows the user to use (and the API to manage/expend) resources according to their level of privilege (e.g., paid users, free users). Possible constraints for security reasons as well---perhaps certain API keys sent in the header correspond with security policies for the API (granat least amount of access needed, user vs admin, etc.).

2. `messages` field as an array suggests memory---as conversations continue, model consumes them for context/improved responses with more interactions. Logging for tracing/history as well (?)

3. `choices` as an array---