# Toolhouse — Scraped Documentation

**Source:** https://toolhouse.ai
**Pages scraped:** 15
**Total characters:** 69758

---

## Toolhouse

**URL:** https://docs.toolhouse.ai

Toolhouse
Toolhouse
is a Backend-as-a-Service (BaaS) to build, run, and manage AI agents. Toolhouse simplifies the process of building agents in a local environment and running them in production.
With Toolhouse, you define agents as code and deploy them as APIs using a single command. Toolhouse agents are automatically connected to the Toolhouse MCP Server; it gives agents access to RAG, memory, code execution, browser use, and all the functionality agents need to perform actions. You can add MCP Servers and even define custom code that the agent can use to perform actions not covered by public MCP Servers.
Toolhouse has built-in eval, prompt optimization, and agentic orchestration.
Features
Agentic Backend-as-a-Service
Toolhouse allows you to define agents and deploy them as APIs. Our backend integrates curated MCP servers, RAG, memory, and all the other tools you need to ship useful agents. Our globally distributed runtime is built for high availability and ensures consistent performance.
Running Agents asynchronously
Local Development
th
is the Toolhouse CLI. It allows you to create, run, and manage agents directly from your favorite environment. You can integrate
th
in your CI/CD pipeline to test and deploy agents with the rest of your code.
⚡
Quick start for developers (CLI)
Schedules
Schedules are Toolhouse's cron service. You can schedule agents to run a specific intervals using cron expressions.
Schedule autonomous runs
Observability
You can inspect each agent run. Agent Logs show you the entire execution lifecycle of each agent you run. For a deeper drill down, you can use Execution logs to check the MCP servers called by your agents.
Execution logs
Toolhouse SDK
The Toolhouse SDK allows you to run Toolhouse in your existing codebase. You can use the SDK if you want to use your own custom agents. The SDK integrates with Vercel AI and LlamaIndex.
Next
Quick start for vibe coders
Last updated
3 months ago
Features
Agentic Backend-as-a-Service
Local Development
Schedules
Observability
Toolhouse SDK

---

## Running Agents asynchronously | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/agent-workers/running-agents-asynchronously

Running Agents asynchronously | Toolhouse
Toolhouse allows you to deploy agents as an API service. This functionality is called
Agent Runs
.
Any agent you build in Agent Studio is automatically deployed as an API; this allows you to use your agents in a repeatable way by calling their dedicated API endpoint.
Agent Runs This feature is particularly useful because it allows you to build and deploy agents directly from your application without needing direct access to an AI model or requiring the adoption of additional frameworks and APIs.
When the agent is done, it can optionally send an HTTP request to a URL such as a webhook or a callback you specify.
Agent Runs are equipped with all the default Toolhouse MCP servers. You can optionally specify a
bundle
to further tailor the functionality of your agent.
IMPORTANT: Agent run uses only the first message of a conversation as the prompt. To ensure proper agent execution, make sure your initial message provides comprehensive instructions and examples.
Creating an Agent Run
Make sure you have your API Key ready. You can see or create an API key in the
API Keys page
.
Head over to Agent Studio and create a chat. If you're looking for inspiration, you can use
this prompt
.
Click the Share button, then copy the Chat ID.
Agent tokens are free
All Agents Runs use a premium LLM model. For a limited time, LLM tokens are free, meaning you will
not
need to worry about cost per token. Your account will only be charged for Toolhouse Execs.
Create an Agent Run using the Agent Runs API:
Python
JavaScript
cURL
Copy
import
requests
import
json
def
send_request
(
toolhouse_api_key
,
chat_id
):
url
=
"
https://api.toolhouse.ai/v1/agent-runs
"
headers
=
{
"
Authorization
"
:
f
"Bearer
{
toolhouse_api_key
}
"
,
"
Content-Type
"
:
"
application/json
"
}
#
Set the JSON payload
payload
=
{
"
chat_id
"
:
chat_id
,
"
vars
"
:
{
"
name
"
:
"
John Doe
"
}
}
#
Send the POST request
response
=
requests
.
post
(
url
,
headers
=
headers
,
json
=
payload
)
return
response
#
Example usage
if
__name__
==
"
__main__
"
:
toolhouse_api_key
=
"
YOUR_TOOLHOUSE_API_KEY
"
chat_id
=
"
YOUR_CHAT_ID
"
response
=
send_request
(
toolhouse_api_key
,
chat_id
)
#
Check if the request was successful
if
response
.
status_code
==
200
:
print
(
"
Request successful. Response:
"
)
print
(
response
.
json
())
else
:
print
(
"
Request failed with status code
"
,
response
.
status_code
)
Your Agent Run will be queued for execution:
Checking an Agent Run status
Use the Get Agent Runs endpoint to retrive the status of a run.
Python
JavaScript
cURL
You will see the run status and its results. The run status will be one of these values:
Status
Meaning
queued
The Agent Run is being scheduled and will be executed shortly.
in_progress
The agent is currently running and its results are being generated
completed
The Agent Run completed successfully. You will be able to see the results of the run in the
results
field of the API response.
failed
The Agent Run completed with one or more errors. You will be able to see the errors in the
results
field of the API response.
This is what a response will look like:
Queued
In progress
Completed
Failed
Continuing an Agent Run
If an Agent Run completed successfully, you can add further messages to it. This will cause the Agent Run to be queued and start again. The agent will keep its context.
Python
JavaScript
cURL
Previous
Agent workers
Next
API Reference
Last updated
1 year ago
Creating an Agent Run
Checking an Agent Run status
Continuing an Agent Run
Copy
async function createAgentRun(toolhouseApiKey, chatId) {
try {
const url = 'https://api.toolhouse.ai/v1/agent-runs';
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
'Content-Type': 'application/json',
};
const body = JSON.stringify({
'chat_id': chatId,
'vars': {
'name': 'John Doe'
}
});
const response = await fetch(url, {
method: 'POST',
headers: headers,
body: body,
});
if (response.ok) {
const data = await response.json();
console.log(data);
} else {
console.error('Failed to retrieve data:', response.status, response.statusText);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const chatId = 'your_chat_id';
await createAgentRun(toolhouseApiKey, chatId);
Copy
curl "https://api.toolhouse.ai/v1/agent-runs" \
-H "Authorization: Bearer $YOUR_TOOLHOUSE_API_KEY" \
--json '{
"chat_id": "your_chat_id",
"vars": { "name": "John Doe" }
}'
Copy
{
"data": {
"id": "bcb7e865-9e6c-xxxx-xxxx-c0110fdf5c2e",
"user_id": "665618fbdeadd84260aa",
"status": "queued",
"results": [],
"created_at": "2025-01-15T01:46:23.568338",
"updated_at": "2025-01-15T01:46:23.568338",
"bundle": "default",
"toolhouse_id": "default"
}
}
Copy
import requests
TOOLHOUSE_API_KEY = "your_toolhouse_api_key"
YOUR_RUN_ID = "your_run_id_here"  # replace with your actual run ID
url = f"https://api.toolhouse.ai/v1/agent-runs/{YOUR_RUN_ID}"
headers = {
"Authorization": f"Bearer {TOOLHOUSE_API_KEY}",
"Content-Type": "application/json"
}
response = requests.get(url, headers=headers)
# Check if the request was successful
if response.status_code == 200:
print("Request successful. Response:")
print(response.json())
else:
print("Request failed with status code", response.status_code, response.text)
Copy
async function getAgentRun(toolhouseApiKey, runId) {
try {
const url = `https://api.toolhouse.ai/v1/agent-runs/${runId}`;
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
'Content-Type': 'application/json',
};
const response = await fetch(url, { headers });
if (response.ok) {
const data = await response.json();
console.log(data);
} else {
console.error('Failed to retrieve data:', response.status, response.statusText);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const chatId = 'your_chat_id';
const status = await getAgentRuns(toolhouseApiKey, chatId);
console.log(status);
Copy
curl "https://api.toolhouse.ai/v1/agent-runs/$YOUR_RUN_ID"
Copy
{
"data": {
"id": "bcb7e865-9e6c-xxxx-xxxx-c0110fdf5c2e",
"chat_id": "a3a26d33-6f4d-4490-a56f-e17f867ba451",
"callback_url": "https://webhooks.example.com/my-webhook",
"status": "queued",
"results": [],
"last_agent_message": null,
"created_at": "2025-01-15T01:46:23.568338",
"updated_at": "2025-01-15T01:46:59.270078",
"bundle": "default",
"toolhouse_id": "default"
}
}
Copy
{
"data": {
"id": "bcb7e865-9e6c-xxxx-xxxx-c0110fdf5c2e",
"chat_id": "a3a26d33-6f4d-4490-a56f-e17f867ba451",
"callback_url": "https://webhooks.example.com/my-webhook",
"status": "in_progress",
"results": [],
"last_agent_message": null,
"created_at": "2025-01-15T01:46:23.568338",
"updated_at": "2025-01-15T01:46:59.270078",
"bundle": "default",
"toolhouse_id": "default"
}
}
Copy
{
"data": {
"id": "bcb7e865-9e6c-xxxx-xxxx-c0110fdf5c2e",
"chat_id": "a3a26d33-6f4d-4490-a56f-e17f867ba451",
"callback_url": "https://webhooks.example.com/my-webhook",
"status": "completed",
"results": [
{
"role": "user",
"content": "Generate a random pet fact and send it to me via email. Here's how to generate a pet fact: \n\n1. Look into memory for past pet facts I already know about. Ensure you avoid mentioning these facts, so I can always get new pet facts. It's okay if I don't have previous pet facts.\n2. Prepare a few sentences about this pet fact. Make it fun and insightful!\n3. Get my email address from memory and send an email with the pet fact. Format it nicely with HTML and CSS so it looks super cute. If you can't find my email in memory, ask me for one.\n4. Store my email and the pet fact you generated in memory."
},
{
"role": "assistant",
"content": [
{
"text": "I'll help you generate and send a random pet fact! Let me follow your steps:\n\n1. First, let me check memory for any past pet facts and your email address:",
"type": "text"
},
{
"args": {},
"type": "tool-call",
"toolName": "memory_fetch",
"toolCallId": "toolu_01L1KXeHYNKhSxqAtLMDdC57"
}
]
},
{
"role": "tool",
"content": [
{
"type": "tool-result",
"result": "<memory created_at=\"2024-12-10 02:52:59\">\n        Email:
[email protected]
\nMeal preferences: vegetarian\n    </memory><memory created_at=\"2024-12-10 03:18:12\">\n        Email:
[email protected]
\nMeal preferences: vegetarian\n    </memory><memory created_at=\"2024-12-10 19:18:08\">\n        Daily Meal Plan for December 10, 2024:\n\n**Breakfast:** \n- **Vegetarian Avocado Toast**  \n  *Ingredients:*  \n  - 1 slice of whole grain bread  \n  - 1 ripe avocado  \n  - Salt, to taste  \n  - Pepper, to taste  \n  - Optional: cherry tomatoes, radishes, or lemon juice \n  \n  *Instructions:*  \n  1. Toast the bread until golden brown.  \n  2. Mash the avocado in a bowl and season with salt and pepper.  \n  3. Spread the mashed avocado over the toasted bread.  \n  4. Top with cherry tomatoes, radishes, or a squeeze of lemon if desired.  \n  \n\n**Lunch:**  \n- **Chickpea Salad**  \n  *Ingredients:*  \n  - 1 can chickpeas, rinsed and drained  \n  - 1 cucumber, chopped  \n  - 1 bell pepper, chopped  \n  - 1/4 red onion, chopped  \n  - 2 tablespoons olive oil  \n  - Juice of 1 lemon  \n  - Salt and pepper, to taste  \n  - Fresh parsley, chopped  \n  \n  *Instructions:*  \n  1. In a large bowl, combine chickpeas, cucumber, bell pepper, and red onion.  \n  2. In a small bowl, whisk together olive oil, lemon juice, salt, and pepper.  \n  3. Pour the dressing over the salad and toss to combine.  \n  4. Garnish with fresh parsley.  \n  \n\n**Dinner:**  \n- **Vegetarian Stir-Fry**  \n  *Ingredients:*  \n  - 1 cup broccoli florets  \n  - 1 cup bell peppers, sliced  \n  - 1 cup carrots, sliced  \n  - 1 cup tofu, cubed  \n  - 2 tablespoons soy sauce  \n  - 1 tablespoon sesame oil  \n  - 2 cloves garlic, minced  \n  - Cooked rice or quinoa, to serve  \n  \n  *Instructions:*  \n  1. Heat sesame oil in a large pan over medium heat.  \n  2. Add garlic and tofu, and stir-fry until golden.  \n  3. Add broccoli, bell peppers, and carrots. Stir-fry until veggies are tender.  \n  4. Add soy sauce and stir to combine.  \n  5. Serve hot on a bed of rice or quinoa.\n    </memory><memory created_at=\"2024-12-10 19:21:53\">\n        Daily Meal Plan for December 10, 2024:\n\n**Breakfast:** \n- **Vegetarian Avocado Toast**  \n  *Ingredients:*  \n  - 1 slice of whole grain bread  \n  - 1 ripe avocado  \n  - Salt, to taste  \n  - Pepper, to taste  \n  - Optional: cherry tomatoes, radishes, or lemon juice   \n  \n  *Instructions:*  \n  1. Toast the bread until golden brown.  \n  2. Mash the avocado in a bowl and season with salt and pepper.  \n  3. Spread the mashed avocado over the toasted bread.  \n  4. Top with cherry tomatoes, radishes, or a squeeze of lemon if desired.  \n   \n\n**Lunch:**  \n- **Chickpea Salad**  \n  *Ingredients:*  \n  - 1 can chickpeas, rinsed and drained  \n  - 1 cucumber, chopped  \n  - 1 bell pepper, chopped  \n  - 1/4 red onion, chopped  \n  - 2 tablespoons olive oil  \n  - Juice of 1 lemon  \n  - Salt and pepper, to taste  \n  - Fresh parsley, chopped  \n  \n  *Instructions:*  \n  1. In a large bowl, combine chickpeas, cucumber, bell pepper, and red onion.  \n  2. In a small bowl, whisk together olive oil, lemon juice, salt, and pepper.  \n  3. Pour the dressing over the salad and toss to combine.  \n  4. Garnish with fresh parsley.  \n   \n\n**Dinner:**  \n- **Vegetarian Stir-Fry**  \n  *Ingredients:*  \n  - 1 cup broccoli florets  \n  - 1 cup bell peppers, sliced  \n  - 1 cup carrots, sliced  \n  - 1 cup tofu, cubed  \n  - 2 tablespoons soy sauce  \n  - 1 tablespoon sesame oil  \n  - 2 cloves garlic, minced  \n  - Cooked rice or quinoa, to serve  \n  \n  *Instructions:*  \n  1. Heat sesame oil in a large pan over medium heat.  \n  2. Add garlic and tofu, and stir-fry until golden.  \n  3. Add broccoli, bell peppers, and carrots. Stir-fry until veggies are tender.  \n  4. Add soy sauce and stir to combine.  \n  5. Serve hot on a bed of rice or quinoa.\n    </memory><memory created_at=\"2024-12-12 06:06:24\">\n        Email:
[email protected]
\nMeal preference: vegetarian\n    </memory><memory created_at=\"2024-12-12 16:09:41\">\n        Email:
[email protected]
\nMeal preferences: vegetarian or vegan\n    </memory><memory created_at=\"2024-12-13 17:14:42\">\n        Email:
[email protected]
\nNews Preferences: Artificial Intelligence and Robotics\n    </memory><memory created_at=\"2024-12-13 22:12:07\">\n        Email:
[email protected]
\nMeal Preferences: Keto, high performance for weightlifting\n    </memory><memory created_at=\"2024-12-13 22:26:26\">\n        Email:
[email protected]
\nMeal preference: Ketogenic\n    </memory><memory created_at=\"2024-12-14 01:27:48\">\n        Email:
[email protected]
\nMeal Preferences: vegetarian\n    </memory><memory created_at=\"2024-12-14 01:31:43\">\n        User's email:
[email protected]
\n    </memory><memory created_at=\"2024-12-14 01:31:45\">\n        User's valid X account: @toolhouseai\n    </memory><memory created_at=\"2025-01-14 23:52:21\">\n        Pet fact shared on December 14, 2023: Capybara fact - World's largest rodent weighing up to 140 pounds. Known for their friendly nature and being called \"nature's ottoman\".\n    </memory><memory created_at=\"2025-01-14 23:53:26\">\n        Pet fact about capybaras (world's largest rodent) shared with
[email protected]
\n    </memory>",
"toolName": "memory_fetch",
"toolCallId": "toolu_01L1KXeHYNKhSxqAtLMDdC57"
}
]
},
{
"role": "assistant",
"content": [
{
"text": "I see your email is
[email protected]
, and I found one previous pet fact about capybaras. Let me search for an interesting new pet fact:",
"type": "text"
},
{
"args": {
"query": "interesting unique pet facts animals",
"num_results": 3
},
"type": "tool-call",
"toolName": "web_search",
"toolCallId": "toolu_01SGidsQaEsE5jTj2cM8Stxx"
}
]
},
{
"role": "tool",
"content": [
{
"type": "tool-result",
"result": "Position: 1\nTitle: Fun Facts\nLink: https://animalmedical.org/fun-facts\nSnippet: Animal Facts · Fleas can jump 350 times its body length. · Hummingbirds are the only birds that can fly backwards. · Crocodiles cannot stick their tongue out.\nSource: animalmedical.org\n\nPosition: 2\nTitle: Pet Fun Facts | Interesting Facts & Trivia on Dogs, Cats & ...\nLink: https://www.petinsurance.com/healthzone/ownership-adoption/pet-ownership/pet-owner-topics/fun-facts-about-pets/\nSnippet: Fun Facts About Birds and Exotic Pets. 68parrot. To survive, every bird must eat at least half its own weight in food each day. A bird's heart beats ...\nSource: Nationwide Pet Insurance",
"toolName": "web_search",
"toolCallId": "toolu_01SGidsQaEsE5jTj2cM8Stxx"
}
]
},
{
"role": "assistant",
"content": [
{
"text": "I'll create a fascinating pet fact about birds! Let me send this via email with a nice format:",
"type": "text"
},
{
"args": {
"to": "
[email protected]
",
"content": "<!DOCTYPE html>\n<html>\n<body style=\"font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto; padding: 20px; background-color: #f5f5f5;\">\n    <div style=\"background-color: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);\">\n        <h1 style=\"color: #2c3e50; text-align: center;\">🐦 Amazing Bird Fact of the Day! 🐦</h1>\n        \n        <div style=\"border-left: 4px solid #3498db; padding-left: 15px; margin: 20px 0;\">\n            <p style=\"font-size: 16px; line-height: 1.6; color: #34495e;\">\n                Did you know that a bird's heart is truly remarkable? Birds must maintain an incredibly active metabolism to survive, and their hearts work extra hard to support this. In fact, every bird needs to eat at least half its own weight in food each day just to keep up with their energy needs!\n            </p>\n            <p style=\"font-size: 16px; line-height: 1.6; color: #34495e;\">\n                This is because birds have extremely fast-beating hearts and high metabolic rates to support their flying abilities and ma

[SCRAPER NOTE: This page was truncated by the scraper due to length. Content continues on the live page.]

---

## Quick start for developers (CLI) | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/quick-start-for-developers-cli

Quick start for developers (CLI) | Toolhouse
This guide will show you how to create and deploy a new agent using the
th
command line interface.
Initial setup
First, install the Toolhouse CLI:
Copy
npm i -g @toolhouseai/cli
The CLI will need a Toolhouse API key. Type
th login
to automatically get one. This command will open your default browser and ask you to log into Toolhouse or create a new account, if you haven't done so already.
Note:
you will only need to run this command once.
Copy
th
login
Your API Key will be stored in the
~/.toolhouse
file.
Other ways to login
As an alternative you can pass an API Key by doing one of these actions:
Manually create a
~/.toolhouse
file and add
TOOLHOUSE_API_KEY=(your API Key)
Set a
TOOLHOUSE_API_KEY
environment variable with your API Key.
Step 0: Create your th file
You can now create a Toolhouse agent file, or
th file
for short. A th file is a YAML file containing the setup for your agent. Type
th new
to create a new agent file:
Copy
th
new
hello.yaml
The CLI will create a
hello.yaml
file in your current folder.
Step 1: Test your agent
You can test your agent by running it via
th run
.
Running the agent will show the configuration parameters and will stream any MCP server calls and the final output. You can change the th file and run
th run
again to see new results.
Once you're happy with the results, you can deploy your agent.
Step 2: Deploy your agent
You can deploy your agent by typing
th deploy
. You agent will be deployed as an API with its own unique URL:
You will receive a URL similar to this:
You can simply call your agent as an HTTP
POST
request, and it will stream the response using the configuration from your th file.
cURL
fetch
httpx
requests
Agents are public on the Free plan
Free plan users can only create public agents.
Pro users can set the visibility of their agents to Private before deploying.
Learn more
Previous
Quick start for vibe coders
Next
Automatically connect MCP servers
Last updated
9 months ago
Initial setup
Step 0: Create your th file
Step 1: Test your agent
Step 2: Deploy your agent
Copy
th run hello.yaml
Copy
th deploy hello.yaml
Copy
https://agents.toolhouse.ai/a1d93c2e-7013-4cea-b857-a27980a52ba2
Copy
curl -XPOST https://agents.toolhouse.ai/a1d93c2e-7013-4cea-b857-a27980a52ba2
Copy
// Remember to run npm i fetch-event-stream
import { events, stream } from 'fetch-event-stream';
const agent = 'https://agents.toolhouse.ai/a1d93c2e-7013-4cea-b857-a27980a52ba2';
const abort = new AbortController();
const response = await fetch(agent, { method: 'POST' });
if (response.ok) {
const stream = events(res, abort.signal);
for await (let event of stream) {
process.stdout.write(event.data);
}
}
Copy
import httpx
import asyncio
agent_url = 'https://agents.toolhouse.ai/a1d93c2e-7013-4cea-b857-a27980a52ba2'
async def main():
async with httpx.AsyncClient() as client:
async with client.stream('POST', agent_url) as response:
if response.status_code == 200:
async for chunk in response.aiter_bytes():
print(chunk.decode(), end='')
asyncio.run(main())
Copy
import requests
agent = 'https://agents.toolhouse.ai/a1d93c2e-7013-4cea-b857-a27980a52ba2'
response = requests.post(agent, stream=True)
response.raise_for_status()
for chunk in response.iter_content(decode_unicode=True):
print(chunk, end='', flush=True)

---

## Schedule autonomous runs | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/agent-workers/schedule-autonomous-runs

Schedule autonomous runs | Toolhouse
You can schedule an agent to run a specific intervals using
Scheduled Runs.
Each agent can have multiple schedules, allowing you to run it the same agent at multiple intervals or for each one of your users.
Each Scheduled Run creates an
Agent Run
, so you can expect Schedules to work similarly. In fact, the difference between Scheduled Runs and Agent Runs is that Agent Runs run once immediately, while Scheduled Runs run at a cadence. Just like Agent Runs, you can optionally specify a
bundle
to further tailor the functionality of your agent, as well as a
Toolhouse ID metadata.
Getting started with Scheduled Runs
Make sure you have your API Key ready. You can see or create an API key in the
API Keys page
.
Specifying a cadence
The minimum cadence for a Scheduled Run is every 10 minutes. If you specify a shorter cadence, your agent will still run at the minimum cadence.
You'll need to specify how often you want to agent to run. The minimum cadence is every 10 minutes. To specify a cadence, simply provide a cron expression or a plain text description of the cadence. Scheduled Runs has a dedicated endpoint to translate plain text to cron:
Python
JavaScript
cURL
Copy
import
requests
import
json
def
send_request
(
toolhouse_api_key
,
cadence
):
params
=
{
"
cron
"
:
cadence
}
url
=
"
https://api.toolhouse.ai/v1/schedules/text-to-cron
"
headers
=
{
"
Authorization
"
:
f
"Bearer
{
toolhouse_api_key
}
"
,
}
response
=
requests
.
get
(
url
,
params
=
params
,
headers
=
headers
,
json
=
payload
)
return
response
#
Example usage
if
__name__
==
"
__main__
"
:
toolhouse_api_key
=
"
YOUR_TOOLHOUSE_API_KEY
"
cadence
=
"
sundays at 5pm UTC
"
response
=
send_request
(
toolhouse_api_key
,
cadence
)
#
Check if the request was successful
if
response
.
status_code
==
200
:
print
(
"
Request successful. Response:
"
)
print
(
response
.
json
())
else
:
print
(
"
Request failed with status code
"
,
response
.
status_code
)
This will return a cron expression you can use in subsequent requests.
Creating a Scheduled Run
Agent tokens are free
All Scheduled Runs use a premium LLM model. For a limited time, LLM tokens are free, meaning you will
not
need to worry about cost per token. Your account will only be charged for Toolhouse Execs.
Head over to Agent Studio and create a chat. If you're looking for inspiration, you can use
this prompt
.
Click the Share button, then copy the Chat ID.
Create an Scheduled Run using the Scheduled Runs API:
Python
JavaScript
cURL
Your Scheduled Run entry will be created.
Changing the cadence of a Scheduled Run
You can change the cadence of a Scheduled Run at any time.
Python
JavaScript
cURL
Pausing or resuming a Scheduled Run
When you pause a Scheduled Run, you interrupt its scheduled cadence without deleting it by setting its
active
field to
false
. This is useful to keep the configuration of your Scheduled Run without deleting it completely. You can resume its execution at any time by changing its
active
field back to
true
.
Any currently active Runs will pause at the end of their execution.
Python
JavaScript
cURL
Deleting a Scheduled Run
Lastly, you can completely delete a Scheduled Run. Any currently active Runs will stop running at the end of their execution.
Python
JavaScript
cURL
Previous
API Reference
Next
API Reference
Last updated
12 months ago
Getting started with Scheduled Runs
Specifying a cadence
Creating a Scheduled Run
Changing the cadence of a Scheduled Run
Pausing or resuming a Scheduled Run
Deleting a Scheduled Run
Copy
async function createAgentRun(toolhouseApiKey, cadence) {
try {
const url = new URL('https://api.toolhouse.ai/v1/schedules/text-to-cron');
url.searchParams.set('cron', cadence);
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
};
const response = await fetch(url, { headers });
if (response.ok) {
const data = await response.json();
console.log(data);
} else {
console.error('Failed to retrieve data:', response.status, response.statusText);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const cadence = 'sundays 5pm UTC';
await createAgentRun(toolhouseApiKey, cadence);
Copy
curl "https://api.toolhouse.ai/v1/schedules/text-to-cron?cron=sundays+5pm+UTC" \
-H "Authorization: Bearer $YOUR_TOOLHOUSE_API_KEY"
Copy
{
"data": "0 17 * * 0"
}
Copy
import requests
import json
def send_request(toolhouse_api_key, chat_id):
url = "https://api.toolhouse.ai/v1/schedules"
headers = {
"Authorization": f"Bearer {toolhouse_api_key}",
"Content-Type": "application/json"
}
# Set the JSON payload
payload = {
"chat_id": chat_id,
"cadence": "0 17 * * 0",
}
# Send the POST request
response = requests.post(url, headers=headers, json=payload)
return response
# Example usage
if __name__ == "__main__":
toolhouse_api_key = "YOUR_TOOLHOUSE_API_KEY"
chat_id = "YOUR_CHAT_ID"
response = send_request(toolhouse_api_key, chat_id)
# Check if the request was successful
if response.status_code == 200:
print("Request successful. Response:")
print(response.json())
else:
print("Request failed with status code", response.status_code)
Copy
async function createAgentRun(toolhouseApiKey, chatId) {
try {
const url = 'https://api.toolhouse.ai/v1/agent-runs';
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
'Content-Type': 'application/json',
};
const body = JSON.stringify({
'chat_id': chatId,
'cadence': '0 17 * * 0',
});
const response = await fetch(url, {
method: 'POST',
headers: headers,
body: body,
});
if (response.ok) {
const data = await response.json();
console.log(data);
} else {
console.error('Failed to retrieve data:', response.status, response.statusText);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const chatId = 'your_chat_id';
await createAgentRun(toolhouseApiKey, chatId);
Copy
curl "https://api.toolhouse.ai/v1/agent-runs" \
-H "Authorization: Bearer $YOUR_TOOLHOUSE_API_KEY" \
--json '{
"chat_id": "your_chat_id",
"cadence": "0 17 * * 0"
}'
Copy
{
"data": {
"id": "bcb7e865-9e6c-xxxx-xxxx-c0110fdf5c2e",
"created_at": "2025-01-15T01:46:23.568338",
"updated_at": "2025-01-15T01:46:23.568338",
"cadence": "0 17 * * 0",
"active": true,
"bundle": "default",
"toolhouse_id": "default"
}
}
Copy
import requests
import json
def send_request(toolhouse_api_key, schedule_id, cadence):
url = f"https://api.toolhouse.ai/v1/schedules/{schedule_id}"
headers = {
"Authorization": f"Bearer {toolhouse_api_key}",
"Content-Type": "application/json"
}
payload = {
"cadence": cadence
}
response = requests.put(url, headers=headers, json=payload)
return response
# Example usage
if __name__ == "__main__":
toolhouse_api_key = "YOUR_TOOLHOUSE_API_KEY"
schedule_id = "YOUR_SCHEDULE_ID"
cadence = "0 */3 * * *"
response = send_request(toolhouse_api_key, schedule_id, cadence)
# Check if the request was successful
if response.status_code == 200:
print("Request successful. Response:")
print(response.json())
else:
print("Request failed with status code", response.status_code)
Copy
async function updateScheduleCadence(toolhouseApiKey, scheduleId, cadence) {
try {
const url = `https://api.toolhouse.ai/v1/schedules/${scheduleId}`;
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
'Content-Type': 'application/json',
};
const body = JSON.stringify({
'cadence': cadence,
});
const response = await fetch(url, {
method: 'PUT',
headers: headers,
body: body,
});
if (response.ok) {
const data = await response.json();
console.log(data);
} else {
console.error('Failed to update cadence:', response.status, response.statusText);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const scheduleId = 'YOUR_SCHEDULE_ID';
const cadence = '0 */3 * * *';
await updateScheduleCadence(toolhouseApiKey, scheduleId, cadence);
Copy
curl -X PUT \
"https://api.toolhouse.ai/v1/schedules/$YOUR_SCHEDULE_ID" \
-H 'Authorization: Bearer $YOUR_TOOLHOUSE_API_KEY' \
-H 'Content-Type: application/json' \
-d '{"cadence": "0 */3 * * *"}'
Copy
import requests
import json
def send_request(toolhouse_api_key, schedule_id, is_active):
url = f"https://api.toolhouse.ai/v1/agent-runs/{schedule_id}"
headers = {
"Authorization": f"Bearer {toolhouse_api_key}",
"Content-Type": "application/json"
}
payload = {
"active": is_active
}
response = requests.put(url, headers=headers, json=payload)
return response
# Example usage
if __name__ == "__main__":
toolhouse_api_key = "YOUR_TOOLHOUSE_API_KEY"
schedule_id = "YOUR_SCHEDULE_ID"
is_active = False
response = send_request(toolhouse_api_key, schedule_id, is_active)
# Check if the request was successful
if response.status_code == 200:
print("Request successful. Response:")
print(response.json())
else:
print("Request failed with status code", response.status_code)
Copy
async function updateScheduleStatus(toolhouseApiKey, scheduleId, isActive) {
try {
const url = `https://api.toolhouse.ai/v1/agent-runs/${scheduleId}`;
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
'Content-Type': 'application/json',
};
const payload = {
'active': isActive,
};
const response = await fetch(url, {
method: 'PUT',
headers: headers,
body: JSON.stringify(payload),
});
if (response.ok) {
const data = await response.json();
console.log(data);
} else {
console.error('Failed to retrieve data:', response.status, response.statusText);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const scheduleId = 'YOUR_SCHEDULE_ID';
const isActive = false;
updateScheduleStatus(toolhouseApiKey, scheduleId, isActive);
Copy
curl -XPUT \
"https://api.toolhouse.ai/v1/schedules/$YOUR_SCHEDULE_ID" \
-H 'Authorization: Bearer $YOUR_TOOLHOUSE_API_KEY' \
-H 'Content-Type: application/json' \
-d '{"active": false}'
Copy
import requests
import json
def send_request(toolhouse_api_key, schedule_id):
url = f"https://api.toolhouse.ai/v1/agent-runs/{schedule_id}"
headers = {
"Authorization": f"Bearer {toolhouse_api_key}",
}
response = requests.delete(url, headers=headers)
return response
# Example usage
if __name__ == "__main__":
toolhouse_api_key = "YOUR_TOOLHOUSE_API_KEY"
schedule_id = "YOUR_SCHEDULE_ID"
response = send_request(toolhouse_api_key, schedule_id)
# Check if the request was successful
if response.status_code == 200:
print("Request successful. Response:")
print(response.json())
else:
print("Request failed with status code", response.status_code)
Copy
async function cancelScheduleRun(toolhouseApiKey, scheduleId) {
try {
const url = `https://api.toolhouse.ai/v1/agent-runs/${scheduleId}`;
const headers = {
'Authorization': `Bearer ${toolhouseApiKey}`,
};
const response = await fetch(url, {
method: 'DELETE',
headers: headers,
});
if (response.ok) {
console.log('Request successful. Response:', await response.json());
} else {
console.error('Request failed with status code', response.status);
}
} catch (error) {
console.error('An error occurred:', error);
}
}
// Example usage
const toolhouseApiKey = 'YOUR_TOOLHOUSE_API_KEY';
const scheduleId = 'YOUR_SCHEDULE_ID';
await cancelScheduleRun(toolhouseApiKey, scheduleId);
Copy
curl -XDELETE \
"https://api.toolhouse.ai/v1/schedules/$YOUR_SCHEDULE_ID" \
-H 'Authorization: Bearer $YOUR_TOOLHOUSE_API_KEY'

---

## Execution logs | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/execution-logs

Execution logs | Toolhouse
Execution logs
give you a way to inspect any MCP servers called by the LLM, including with their arguments and return values.
Execution logs are useful to understand the calls being made by your agents, as well as the type of arguments. Each execution is timestamped so you can understand execution time and optimize latency accordingly.
Execution logs work out-of-the-box across any agent, so you won't have to add specific instrumentation to your code.
Using Execution logs
Head over to
Execution logs
in your Toolhouse. In the left hand side column, you will see your past executions, tagged by their own call ID as generated by the agent. Click on one execution to see its details on the right hand side of the page.
What data is available for each execution
For each MCP server call, you'll see these details:
Data
Description
Call ID
The ID of the call, as passed by the LLM. This is useful to tell apart two different calls of the same server by the same agent.
Execution timestamp
The date and time of when the server was called. This is computed by Toolhouse.
Duration
The duration of the exeuction. This is determined by measuring the delta between the time Toolhouse invokes the server and the time it returns its output to Toolhouse.
Input
Input arguments as generated by the LLM.
Output
The complete return value of the call.
Last updated
3 months ago
Using Execution logs
What data is available for each execution

---

## Quick start for vibe coders | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/quick-start-for-vibe-coders

Quick start for vibe coders | Toolhouse
This guide will show you how to build your first agent with the Toolhouse Agent Studio.
Step 1: Describe your agent
First,
navigate to Agent Studio.
Then, press
Create new Agent.
You will find a chatbot like interface like this.
In the chat box you can
describe
what you want the agent to do, Toolhouse will
write the agent for you.
Make sure you are
specific
with your demands.
Channel your inner prompt engineer
Agent Studio helps you write the right prompt for your agents. You will just need to start in your own words, adding any details you may feel relevant.
Example of prompts (with breakdown)
Good prompt
Why it’s good:
Specific task (search for sentiment mentions about a product)
Clear outputs (report, sentiment analysis)
Defined interaction (ask about emailing)
Useful for automation or monitoring
Bad prompt
Why it’s bad:
Too vague
: finding competitors is a good start, but being specific could be better (search x, what type of competitors are you looking for”
Undefined scope
: What kind of business help? Marketing, sales, operations?
No clear task or output
: There’s no goal, input, or expected result.
Large Scope “ finding competitors” will bring inconsistent results due to the wide scope the agent will search
No one was born a prompt-engineer on their first day
Remember: you can have a natural conversation with Agent Studio, and add as many details as you can, even after it generates your first prompt.
Give your agent an exit strategy.
For example, you can tell Agent Studio "if the agent is searching the web, limit it to 5 results"
Tell your agent what it should do or should not do.
For example, you can tell Agent Studio: "never show results from apartments.com"
Give your agent a tone, a voice, a personality.
If you need, you can make the agent respond in a particular way. Try telling Agent Studio: "always make the agent ask a followup question"
Step 2: Run your agent
Once you've prompted your agent, you can run it to see how it responds.
When you run the agent it will reply to your first
message
(which you can see at the bottom of the YAML file, usually after
system_prompt
).
The
message
is what the agent will use as instructions when it runs the first time you press
Run
.
Step 3: Publish your agent
If the agent you built is satisfactory, press deploy to make it
live
.
If not, then you can continue chatting with the agent studio to refine it.
At any point you can click
Publish
to publish your agent. Publishing your agent will make it available for use, so you can call the agent directly or integrate it with other tools you're using.
You will find your published agents under the
Agents tab.
Step 4: Integrating your agent
When your agent has been published you can start using it immediately.
Use your agent immediately
Each agent comes with a built-in chat interface, hosted by Toolhouse. This chat allows you and your end users to interact with your agent without the need to build from scratch.
To use it, simply click
Publish
, then click the Agent interface link:
Once you click it you will be redirected to an Agent interface where you can interact with your agent, just like ChatGPT, except it's already connected to all the MCP servers Toolhouse offers by default.
Schedule your Agent for later
Toolhouse allows you to schedule agents so they can run at certain intervals.
To do so, click
Publish,
then can go to the
Schedule
tab.
All you need to do from here is set a schedule for your agent to run and click
Create schedule.
Examples of agents you can schedule
Vibe code your frontend
Navigate to the
Lovable
or
Bolt
tabs under the Agent Deployment Success section, then press the clipboard icon to copy the prompt.
Go to
Lovable
or
Bolt
and paste your prompt, then press Enter.
You will build an app that's automatically connected to Toolhouse. You can continue to build your project from there with Lovable, but your backend is now up and running!
Get Help
You can find help from our
AI support agent
live: our help chatbot can answer any questions.
Alternatively you can
join us on Discord
where the Toolhouse team and the community will help you out.
Previous
Toolhouse
Next
Quick start for developers (CLI)
Last updated
2 months ago
Step 1: Describe your agent
Example of prompts (with breakdown)
Step 2: Run your agent
Step 3: Publish your agent
Step 4: Integrating your agent
Use your agent immediately
Schedule your Agent for later
Vibe code your frontend
Get Help
Copy
Build an agent that searches X for positive, negative, and neutral mentions about my product. The agent should give me a nice report and ask it if I want the report sent via email.
Copy
Build an agent that finds competitors helps me with my business.
Copy
Find me trending AI events in SF and send an email to [insert your email here]

---

## Bundle MCP servers together | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/advanced-concepts/bundles

Bundle MCP servers together | Toolhouse
By default, your agent can see and use all the MCP servers available in Toolhouse. Sometimes you want agents to have access only to specific servers. You can do so with
Bundles.
Bundles
help you define groups of MCP servers you want to pass to the LLM based on specific contextual need of each agent. For example, let's say you need to create an agent whose only role is to search the web for information about certain companies. Instead of using MCP servers available, the agent will only need to use scraping and browsing servers. In cases like this, you can create a Bundle to restrict the agent to only perform those actions.
Bundles are effective in improving accuracy of the agent, because they only give your agent a subset of all the servers available. They're also a good way to keep your context tidy by not exposing unneeded servers.
Bundles are also useful in many other cases:
Environments.
You can create separate Bundles with MCP servers for development, staging, and production.
Provider-specific servers.
You can create Bundles of MCP servers based on a specific model or provider.
Testing and evaluation.
You can create different collections to evaluate the behaviors of an LLM when passing different MCP servers.
By default, all MCP servers in Toolhouse are installed to a default bundle called
default
. If you configure your agent file without a bundle, you're implicitly calling the default bundle.
Create a Bundle
To create a Bundle, go to the
Bundles
page, then click
Create New Bundle
.
Then, give a name to your Bundle. This name can be any string up to 128 characters long. The Bundle name will be the identifier you will pass to
get_tools
to pass the bundle to your LLM.
After choosing a name, you can select the MCP servers you want to add to your Bundle. You will need to choose at least one tool in order to save it.
Once you're done, click
Create.
Use a Bundle
In order to use a Bundle, you need to pass the identifier you created to your agent file. Bundles are case insensitive.
Copy
title
:
News Digest
system_prompt
:
You are a newsletter curator. Search the web for recent, relevant news about the user's query, then send an email to the user.
message
:
What's going on in sports?
bundle
:
only_web_search
Edit a Bundle
You can add or remove MCP servers from a Bundle, or change its name. To edit a Bundle, go to the
Bundles
page.
Locate the Bundle you want to edit, then click
Edit.
Make the requested changes and click
Update.
You won't be able to update a Bundle if you remove all MCP servers from it. Make sure you have at least one MCP server in your Bundle in order to update it.
Delete a Bundle
To delete a Bundle, go to the
Bundles
page. Locate the Bundle you want to delete, then click
Delete.
Confirm the operation to actually delete the Bundle.
Deleting a Bundle cannot be undone. Ensure you are not using the Bundle in production before proceeding.
Naming your Bundles
Bundles are very flexible and you can adapt them to a lot of scenarios. For example, you can use them to separate concerns between environments, for testing and evaluations, or to equip different models with specific sets of MCP servers.
Choosing the right naming format
Name by environment
You can create one Bundle per environment, and name them for example
prod
,
staging
,
dev
. This is useful if you don't need to version your Bundles, and it allows you to test changes on an environment before making changes to the others.
Development
Staging
Production
Name by version
You can create one Bundle per version, for example
v1
,
v2
,
v3
and so on. All Bundles will be shared across environments, but you will still be able to test out your changes in an development environment before promoting to production. This is useful if you use the same MCP servers across each LLM.
Set a configuration variable (for example
TOOLHOUSE_BUNDLE_VERSION
environment variable) in each of your environments. Each environment can point to a different version.
Every time you want make a change, you can create a new Bundle or duplicate the Bundle with the latest version number. Increment the version number on this Bundle; for example, if you are duplicating
v3
, your new Bundle name should be
v4
.
Set the
TOOLHOUSE_BUNDLE_VERSION
variable of your test environment (e.g. development) to this new version.
Make your changes in your BUNDLE.
When you're confident with your changes, set the
TOOLHOUSE_BUNDLE_VERSION
variable in production to this new version.
Group by functionality, version, and environment
You can use a naming convention such as
bundle_name-version-env
, for example
crm_lookup_and_search-v1-dev-alice
. This way you can logically group functionality while also keeping the benefit of versioning and environment choice.
You can also create a bundle for a specific LLM, for example
memory_gpt-4.1-nano
, which allows you to give each LLM different MCP servers. This strategy is effective when some models tend to pick the wrong MCP servers, or hallucinate on a server name.
Previous
Managing secrets
Next
Customize agents for your end users
Last updated
10 months ago
Create a Bundle
Use a Bundle
Edit a Bundle
Delete a Bundle
Naming your Bundles
Choosing the right naming format
Copy
bundle: allowed_tools_dev
Copy
bundle: allowed_tools_staging
Copy
bundle: allowed_tools_prod
Copy
# In dev
bundle: tools_v4
# In prod
bundle: tools_v3

---

## Agent workers | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/agent-workers

Agent workers | Toolhouse
Your agents can become background workers. You can launch them as a one-off process, or schedule them to perform their tasks at a specified interval.
Running Agents asynchronoously with Agent Runs
You can execute an Agent in the background by using
Agent Runs.
You can configure the agent to send its results to a callback URL when done.
Running Agents asynchronously
Executing Agents at specified interval with Schedules
Schedules
is Toolhouse's a built-in cron service. You can run an Agent on a schedule and configure its parameters. Just like Agent Runs, you can configure your Agent to send its results to a callback URL every time it completes each one of its executions.
Schedule autonomous runs
Previous
Generate a Vibe Coding Prompt with Toolhouse CLI
Next
Running Agents asynchronously
Last updated
11 months ago
Running Agents asynchronoously with Agent Runs
Executing Agents at specified interval with Schedules

---

## API Reference | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/agent-workers/running-agents-asynchronously/api-reference

API Reference | Toolhouse
The Bearer Token is the TOOLHOUSE_API_KEY. You can find your API key(s)
here
.
Get All Agent Runs
get
/v1/agent-runs
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Responses
200
Agent runs successfully retrieved
application/json
data
object · AgentRun[]
Required
Show properties
500
Internal server error
get
/v1/agent-runs
HTTP
HTTP
cURL
JavaScript
Python
200
Agent runs successfully retrieved
Create Agent Run
post
/v1/agent-runs
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Body
application/json
application/json
chat_id
string · uuid
Required
schedule_id
any of
Optional
string · uuid
Optional
or
null
Optional
bundle
string
Optional
Default:
default
toolhouse_id
string
Optional
Default:
default
vars
any of
Optional
object
Optional
Show properties
or
null
Optional
callback_url
any of
Optional
string · uri
· min: 1
Optional
or
null
Optional
Responses
200
Agent run successfully created
application/json
any
Optional
403
Chat ID is invalid or does not belong to you
422
Validation Error
application/json
500
Internal server error
post
/v1/agent-runs
HTTP
HTTP
cURL
JavaScript
Python
200
Agent run successfully created
No content
Get Agent Run
get
/v1/agent-runs/
{run_id}
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Path parameters
run_id
string · uuid
Required
Responses
200
Agent run successfully retrieved
application/json
data
object · AgentRunResponse
Required
Show properties
403
Chat ID associated to this Agent Run is invalid
404
Agent run not found
422
Validation Error
application/json
500
Internal server error
get
/v1/agent-runs/
{run_id}
HTTP
HTTP
cURL
JavaScript
Python
200
Agent run successfully retrieved
Put Agent Run
put
/v1/agent-runs/
{run_id}
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Path parameters
run_id
string · uuid
Required
Body
application/json
application/json
message
string
Required
Responses
200
Agent run successfully updated
application/json
any
Optional
403
Chat ID is invalid or does not belong to you
406
You can only add messages to completed runs
422
Validation Error
application/json
500
Internal server error
put
/v1/agent-runs/
{run_id}
HTTP
HTTP
cURL
JavaScript
Python
200
Agent run successfully updated
No content
Previous
Running Agents asynchronously
Next
Schedule autonomous runs
Last updated
9 months ago
GET
Get All Agent Runs
POST
Create Agent Run
GET
Get Agent Run
PUT
Put Agent Run
Copy
GET /v1/agent-runs HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Accept: */*
Copy
{
"data": [
{
"id": "123e4567-e89b-12d3-a456-426614174000",
"chat_id": "123e4567-e89b-12d3-a456-426614174000",
"user_id": "text",
"schedule_id": "123e4567-e89b-12d3-a456-426614174000",
"status": "queued",
"results": [
{
"ANY_ADDITIONAL_PROPERTY": "anything"
}
],
"created_at": "2026-04-20T14:33:31.422Z",
"updated_at": "2026-04-20T14:33:31.422Z",
"bundle": "default",
"toolhouse_id": "default",
"vars": {
"ANY_ADDITIONAL_PROPERTY": "text"
},
"callback_url": "https://example.com",
"error": "text"
}
]
}
Copy
POST /v1/agent-runs HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Content-Type: application/json
Accept: */*
Content-Length: 226
{
"chat_id": "123e4567-e89b-12d3-a456-426614174000",
"schedule_id": "123e4567-e89b-12d3-a456-426614174000",
"bundle": "default",
"toolhouse_id": "default",
"vars": {
"ANY_ADDITIONAL_PROPERTY": "text"
},
"callback_url": "https://example.com"
}
Copy
GET /v1/agent-runs/{run_id} HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Accept: */*
Copy
{
"data": {
"id": "123e4567-e89b-12d3-a456-426614174000",
"chat_id": "123e4567-e89b-12d3-a456-426614174000",
"schedule_id": "123e4567-e89b-12d3-a456-426614174000",
"results": [
{
"ANY_ADDITIONAL_PROPERTY": "anything"
}
],
"status": "queued",
"created_at": "2026-04-20T14:33:31.422Z",
"updated_at": "2026-04-20T14:33:31.422Z",
"bundle": "default",
"toolhouse_id": "default",
"vars": {
"ANY_ADDITIONAL_PROPERTY": "text"
},
"callback_url": "https://example.com",
"last_agent_message": "text"
}
}
Copy
PUT /v1/agent-runs/{run_id} HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Content-Type: application/json
Accept: */*
Content-Length: 18
{
"message": "text"
}

---

## https://docs.toolhouse.ai/cdn-cgi/l/email-protection

**URL:** https://docs.toolhouse.ai/cdn-cgi/l/email-protection

> **Error:** Client error '404 Not Found' for url 'https://docs.toolhouse.ai/cdn-cgi/l/email-protection'
For more information check: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404


---

## Build agents with the th file | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/advanced-concepts/build-agents-with-the-th-file#public

Build agents with the th file | Toolhouse
You can build agents as code using the Toolhouse Agent File, or
th file
for short. The th file is a YAML file that defines the configuration of an agent deployed on the Toolhouse platform. This file contains essential information about the agent, such as its ID, behavior, and interactions. In this documentation, we will walk you through the different sections of the configuration file, explaining the purpose and usage of each entry.
File Structure
The configuration file is a YAML file that consists of several key-value pairs. The file is divided into sections, each defining a specific aspect of the agent.
Agent ID
The first section of the file defines the agent's ID.
id
Type:
String
Description:
The unique identifier of the agent. This value is automatically generated when creating a new agent.
Do not change this value unless you know what you're doing
, as it may cause issues with the agent's functionality.
title
Type:
String
Description:
The name of the agent. This value is used for display purposes and can be changed at any time.
Agent Behavior
The next section defines the agent's behavior.
message
Type:
String
Description:
The prompt that the agent will use to generate responses. This value will be used when run the agent autonomously, and it has the role of a user message. When your agent is deployed, you or your end users can dynamically set this value when calling the agent.
system_prompt
Type:
String
Description:
The system prompt that sets the context for the agent.
vars
Type:
Dictionary
Description:
A dictionary of variables used in the
system_prompt
. You can define default values for these variables. Ensure that the variable names match those used in the
system_prompt
.
Example:
In this example, the
system_prompt
is set to
"Write a short story about {topic} featuring {name}"
, which means the
vars
section must contain
topic
and
name
. When running the agent, Toolhouse will compile the system_prompt by replacing the variables with these values:
You can overwrite the value of one or more of your variables by passing values when making an API request to your agent:
Agent Configuration
The next section defines additional configuration options for the agent.
bundle
Type:
String
Description:
The
bundle configuration
for the agent. You can specify a bundle name
Example:
public
Agents are public on the Free plan
Free plan users can only create public agents.
Pro users can set the visibility of their agents to Private before deploying.
Type:
Boolean
Default:
true
Description:
A flag indicating whether the agent is public or private. Public agents are visible to all users and can be used by anyone. Private agents are only visible to the owner and can only be used when called with the owner's API key. Toolhouse Pro users can create private agents that are not visible to other users.
toolhouse_id
Type:
String
Description:
The end-user ID that provides context about the user interacting with the agent.
MCP - Model Context Protocol
All Toolhouse agents support connecting with HTTP Streamable and SSE remote MCP servers.
mcp_servers
Type:
Array
Description:
A list of one or more MCP server url (must start with https://) that you want your agent to use to perform its duty
Example:
We do not currently support OAuth MCP servers
RAG
The final section defines the RAG folder you want to use for this agent.
rag
Type:
String
Description:
The
RAG folder
you previously created using the
th rag
command.
Examples
Here are a few examples of how you can use the Toolhouse Agent Configuration file.
Product Description Generator
This th file will create an agent that searches the web for a particular product. Using the search results, it will generate a compelling product description.
Marketing Deep Personalization
This agent reads the purchase history for a specific user to create and send a personalized marketing email.
Based on the prompt, the agent will use RAG to retrieve the purchase history for that specific user identified by its ID. When done, it will prepare the marketing email and send it to the user.
Previous
Advanced concepts
Next
Test agents before publishing
Last updated
9 months ago
File Structure
Agent ID
Agent Behavior
Agent Configuration
MCP - Model Context Protocol
RAG
Examples
Copy
system_prompt: "Your task is to write a short story about {topic} featuring {name}"
vars:
topic: "a mysterious island"
name: "John Doe"
Copy
curl -XPOST "https://agents.toolhouse.ai/$YOUR_AGENT_ID"
# Prompt will be "Write a short story about a mysterious island featuring John Doe"
Copy
curl -XPOST "https://agents.toolhouse.ai/$YOUR_AGENT_ID" \
--json '{ "vars": {"topic": "a space odyssey"}, "message": "what story do you have for me today?" }'
# System prompt will be "Write a short story about a space odyssey featuring John Doe"
Copy
curl -XPOST "https://agents.toolhouse.ai/$YOUR_AGENT_ID" \
--json '{ "vars": {"topic": "a space odyssey", "name": "Philip J. Fry"}, "message": "what story do you have for me today?" }'
# System prompt will be "Write a short story about a space odyssey featuring Philip J. Fry"
Copy
bundle: "default"
Copy
mcp_servers:
- https://mcp.toolhouse.ai/@toolhouse/supabase?access_token=sbp_192839128319238123bb5416947c1863334ffd61&project_ref=nvbuionbuviobnviuobnvuinvboui
Copy
title: Product Description Generator
system_prompt: "You are a brilliant marketer for an e-commerce. Your job it so write a compelling product description for the product the user will give you. This descrpition must suitable for the product detail page of an e-commerce website. Your product description will be based on its features and benefits. Use the results from the web search to inform your response. You should aim to write a {length} description."
vars:
length: "long"
message: "Write a description for the latest iPhone at https://apple.com/iphone"
bundle: "default"
public: true
Copy
title: Personalized Marketing Email Generator
system_prompt: |-
You are a skilled marketing email writer. Use the customer's data to generate a personalized email. This is what you need to know about the user:
Customer ID: {customer_id}
Name: {customer_name}
Email: {customer_email}
First, search for {customer_id}'s purchase history in RAG. When done, use the customer's data to inform your response and send the email to {customer_email}.
vars:
customer_id: "6js3ONksv"
customer_name: "John Doe"
customer_email: "
[email protected]
"
message: "Write a personalized marketing email to {customer_name}"
bundle: "default"
rag: purchase_history
public: false

---

## Automatically connect MCP servers | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/automatically-connect-mcp-servers

Automatically connect MCP servers | Toolhouse
Toolhouse now makes it effortless to integrate your agents with external services using
MCP Discovery (Model Context Protocol)
. When you create an agent in
Agent Studio
, Toolhouse will automatically detect which MCP servers your agent needs, giving it the ability to interact with tools like Notion, Linear, Google Calendar, and more.
This functionality is built into Agent Studio, which means you won’t have to change the way you use it.
For example, if you tell Agent Studio that your agent should work with Google Calendar, Toolhouse will automatically connect it to the appropriate MCP server so it can view or edit calendar events.
Getting started with MCP Discovery
In this example, we’ll create an agent that reads a Notion page and creates Linear tickets from it.
Open Agent Studio
Start by writing a natural-language prompt describing what you want your agent to do.
In this case:
“Build a virtual engineering manager who reads a Notion page the user provides and creates a bunch of Linear tickets from it.”
Run the Prompt
After hitting
Run
, Toolhouse will analyze the agent’s needs and display a sidebar showing the recommended MCP servers (e.g., Notion and Linear in this case).
Configure MCP Servers
In the sidebar, enter any required API keys or credentials. These give your agent access to the relevant services and unlock its full capabilities. Required fields will be marked.
Publish Your Agent
Once configured, click
Publish
to deploy your agent. Your agent is now live and fully integrated with the MCP servers you selected—no additional configuration needed.
Enabling and disabling MCP servers
Sometimes,
MCP Discovery
may detect servers you don’t need. You can easily disable them by toggling the
Enabled
switch in the sidebar.
Use MCP discovery alongside your existing MCP servers
You can still import your favorite MCP servers and use them with the servers found by Toolhouse. To do so, simply tell Agent studio “add my MCP server” followed by the URL to the MCP server you want to use.
Your agent now has real-world superpowers: it can pull from Notion, create tickets in Linear, update Google Calendar, and more. Want to connect more services? Just describe what you want your agent to do, and Toolhouse will take care of the rest.
Previous
Quick start for developers (CLI)
Next
Custom MCP servers integrations
Last updated
8 months ago
Getting started with MCP Discovery
Open Agent Studio
Run the Prompt
Configure MCP Servers
Publish Your Agent
Enabling and disabling MCP servers
Use MCP discovery alongside your existing MCP servers

---

## Customize agents for your end users | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/advanced-concepts/using-metadata

Customize agents for your end users | Toolhouse
You can configure your agents to behave in a different way for each one of your end users. This behavior is useful if you're using Memory and you want the same agent to store and retrieve with a user's own memories.
In Toolhouse you accomplish this with
Toolhouse ID.
Toolhouse only supports one type of metadata called
toolhouse_id
. You can use it to set a unique identifier for a user.
Toolhouse does not send metadata to the LLM.
By default, Toolhouse uses a user ID with value
default
. If you don't specify an ID, your agent will assume to be a generic agent that has no context over all the end users of your app.
Setting the Toolhouse ID
To set an ID, you simply need to pass it in your first agent request:
Copy
curl
-XPOST
"
https://agents.toolhouse.ai/
$YOUR_AGENT_ID
"
\
--json
'
{ "message": "what did I eat last summer?", "toolhouse_id": "user_1ee7c0de" }
'
Scoping (Pro or Business plans only)
If you are on a Pro or Business plan, your Toolhouse ID is further separated by the API Key you use to call the agent. This allows your team to test agents in paraller, or to further separate and control who has access to your agent.
For example, suppose you have a user whose ID is
alice
. If you have two Toolhouse API keys (for example production and development), production
alice
will be treated as a separate user than development
alice
:
Copy
curl
-XPOST
"
https://agents.toolhouse.ai/
$YOUR_AGENT_ID
"
\
-H
"
Authorization: Bearer th-123xxxx567
"
--json
'
{ "message": "what did I eat last summer?", "toolhouse_id": "alice" }
'
#
In the following request, the API key changes, so
#
the user "alice" will map to a new user
curl
-XPOST
"
https://agents.toolhouse.ai/
$YOUR_AGENT_ID
"
\
-H
"
Authorization: Bearer th-654xxxx981
"
--json
'
{ "message": "what did I eat last summer?", "toolhouse_id": "alice" }
'
ID privacy and data protection
Hashing is a one-way process that converts input data into a fixed-size string of characters. It's deterministic, meaning the same input always produces the same output, but it's computationally infeasible to reverse.
Toolhouse automatically protects your data and the data of your users by hashing the Toolhouse ID value. This means you can safely pass any user ID you normally use, and Toolhouse will convert it into its hashed representation.
Hashing your username avoids MCP servers (including the Toolhouse built-in server) to see your actual user IDs. For example, if you pass a user value of
[email protected]
, Toolhouse will automatically it into something like
5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8
. Due to the one-way nature of hashing, it is infeasible to convert this long string into its input user ID.
In other words, if you are using emails or other values as IDs, hashing makes it safe to pass them to Toolhouse, because neither Toolhouse nor the MCP servers will be able to decode them. Toolhouse will
never pass
your user IDs to any MCP server, including those built by Toolhouse itself.
Previous
Bundle MCP servers together
Next
Generate a Vibe Coding Prompt with Toolhouse CLI
Last updated
8 months ago
Setting the Toolhouse ID
Scoping (Pro or Business plans only)
ID privacy and data protection

---

## API Reference | Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse/agent-workers/schedule-autonomous-runs/api-reference

API Reference | Toolhouse
Get Schedule List
get
/v1/schedules
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Responses
200
Schedules retrieved successfully
application/json
data
object · Schedule[]
Required
Show properties
500
Internal server error
get
/v1/schedules
HTTP
HTTP
cURL
JavaScript
Python
200
Schedules retrieved successfully
Create Schedule
post
/v1/schedules
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Body
application/json
application/json
chat_id
string · uuid
Required
cadence
string
Required
bundle
string
Optional
Default:
default
toolhouse_id
string
Optional
Default:
default
vars
any of
Optional
object
Optional
Show properties
or
null
Optional
callback_url
any of
Optional
string · uri
· min: 1
Optional
or
null
Optional
Responses
200
Schedule created successfully
application/json
any
Optional
403
Invalid chat ID or chat does not belong to user
422
Validation Error
application/json
500
Internal server error
post
/v1/schedules
HTTP
HTTP
cURL
JavaScript
Python
200
Schedule created successfully
No content
Text To Cron
get
/v1/schedules/text-to-cron
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Query parameters
cron
string
Required
Responses
200
Cron expression generated successfully
application/json
any
Optional
400
Invalid cron text input
422
Validation Error
application/json
500
Internal server error
get
/v1/schedules/text-to-cron
HTTP
HTTP
cURL
JavaScript
Python
200
Cron expression generated successfully
No content
Get Schedule By Id
get
/v1/schedules/
{schedule_id}
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Path parameters
schedule_id
string · uuid
Required
Responses
200
Schedule retrieved successfully
application/json
any
Optional
404
Schedule not found
422
Validation Error
application/json
500
Internal server error
get
/v1/schedules/
{schedule_id}
HTTP
HTTP
cURL
JavaScript
Python
200
Schedule retrieved successfully
No content
Edit Schedule
put
/v1/schedules/
{schedule_id}
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Path parameters
schedule_id
string · uuid
Required
Body
application/json
application/json
active
boolean
Optional
Default:
true
cadence
string
Required
bundle
any of
Optional
Default:
default
string
Optional
or
null
Optional
toolhouse_id
any of
Optional
Default:
default
string
Optional
or
null
Optional
vars
any of
Optional
object
Optional
Show properties
or
null
Optional
chat_id
any of
Optional
string · uuid
Optional
or
null
Optional
callback_url
any of
Optional
string · uri
· min: 1
Optional
or
null
Optional
Responses
200
Schedule updated successfully
application/json
any
Optional
422
Validation Error
application/json
500
Internal server error
put
/v1/schedules/
{schedule_id}
HTTP
HTTP
cURL
JavaScript
Python
200
Schedule updated successfully
No content
Del Schedule
delete
/v1/schedules/
{schedule_id}
Authorizations
HTTPBearer
HTTPBearer
Authorization
string
Required
Bearer authentication header of the form Bearer <token>.
Path parameters
schedule_id
string · uuid
Required
Responses
200
Schedule deleted successfully
application/json
any
Optional
400
Schedule not found
422
Validation Error
application/json
500
Internal server error
delete
/v1/schedules/
{schedule_id}
HTTP
HTTP
cURL
JavaScript
Python
200
Schedule deleted successfully
No content
Previous
Schedule autonomous runs
Next
Affiliates
Last updated
1 year ago
GET
Get Schedule List
POST
Create Schedule
GET
Text To Cron
GET
Get Schedule By Id
PUT
Edit Schedule
DELETE
Del Schedule
Copy
GET /v1/schedules HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Accept: */*
Copy
{
"data": [
{
"id": "123e4567-e89b-12d3-a456-426614174000",
"user_id": "text",
"chat_id": "123e4567-e89b-12d3-a456-426614174000",
"created_at": "2026-04-20T14:43:15.296Z",
"updated_at": "2026-04-20T14:43:15.296Z",
"last_ran_at": "2026-04-20T14:43:15.296Z",
"active": true,
"archived": false,
"cadence": "text",
"bundle": "default",
"toolhouse_id": "default",
"vars": {
"ANY_ADDITIONAL_PROPERTY": "text"
},
"title": "text",
"chat_archived": true,
"callback_url": "https://example.com"
}
]
}
Copy
POST /v1/schedules HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Content-Type: application/json
Accept: */*
Content-Length: 190
{
"chat_id": "123e4567-e89b-12d3-a456-426614174000",
"cadence": "text",
"bundle": "default",
"toolhouse_id": "default",
"vars": {
"ANY_ADDITIONAL_PROPERTY": "text"
},
"callback_url": "https://example.com"
}
Copy
GET /v1/schedules/text-to-cron?cron=text HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Accept: */*
Copy
GET /v1/schedules/{schedule_id} HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Accept: */*
Copy
PUT /v1/schedules/{schedule_id} HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Content-Type: application/json
Accept: */*
Content-Length: 204
{
"active": true,
"cadence": "text",
"bundle": "default",
"toolhouse_id": "default",
"vars": {
"ANY_ADDITIONAL_PROPERTY": "text"
},
"chat_id": "123e4567-e89b-12d3-a456-426614174000",
"callback_url": "https://example.com"
}
Copy
DELETE /v1/schedules/{schedule_id} HTTP/1.1
Authorization: Bearer YOUR_SECRET_TOKEN
Accept: */*

---

## Toolhouse

**URL:** https://docs.toolhouse.ai/toolhouse

Toolhouse
Toolhouse
is a Backend-as-a-Service (BaaS) to build, run, and manage AI agents. Toolhouse simplifies the process of building agents in a local environment and running them in production.
With Toolhouse, you define agents as code and deploy them as APIs using a single command. Toolhouse agents are automatically connected to the Toolhouse MCP Server; it gives agents access to RAG, memory, code execution, browser use, and all the functionality agents need to perform actions. You can add MCP Servers and even define custom code that the agent can use to perform actions not covered by public MCP Servers.
Toolhouse has built-in eval, prompt optimization, and agentic orchestration.
Features
Agentic Backend-as-a-Service
Toolhouse allows you to define agents and deploy them as APIs. Our backend integrates curated MCP servers, RAG, memory, and all the other tools you need to ship useful agents. Our globally distributed runtime is built for high availability and ensures consistent performance.
Running Agents asynchronously
Local Development
th
is the Toolhouse CLI. It allows you to create, run, and manage agents directly from your favorite environment. You can integrate
th
in your CI/CD pipeline to test and deploy agents with the rest of your code.
⚡
Quick start for developers (CLI)
Schedules
Schedules are Toolhouse's cron service. You can schedule agents to run a specific intervals using cron expressions.
Schedule autonomous runs
Observability
You can inspect each agent run. Agent Logs show you the entire execution lifecycle of each agent you run. For a deeper drill down, you can use Execution logs to check the MCP servers called by your agents.
Execution logs
Toolhouse SDK
The Toolhouse SDK allows you to run Toolhouse in your existing codebase. You can use the SDK if you want to use your own custom agents. The SDK integrates with Vercel AI and LlamaIndex.
Next
Quick start for vibe coders
Last updated
3 months ago
Features
Agentic Backend-as-a-Service
Local Development
Schedules
Observability
Toolhouse SDK
