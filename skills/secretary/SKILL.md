---
name: secretary
description: Sets up GetNod, a project secretary. It creates one private page in the person's own Claude account, where the chat records their decisions, tasks and questions and they answer from their phone. Use when the person says "Install GetNod", "Set up GetNod", "Set up a project secretary", "Установи GetNod", "Поставь секретаря", or asks how GetNod works or how to remove it. Do not use for other note-taking or task requests.
---

# GetNod: project secretary

GetNod gives one Claude project one page. On that page the chat records what the person decided, what they need to do and what they need to answer. The person answers with buttons, ticks tasks and leaves notes from their phone. Every new chat in the project reads the page and carries on.

Files next to this one:

- The page is not in the plugin: at installation you download it from GetNod's public GitHub repository, at a fixed version, and check its checksum (step 1).
- `references/project-instruction-en.md`: the project instruction in English.
- `references/project-instruction-ru.md`: the same instruction in Russian.

Speak the person's language. Use the Russian instruction for a person who writes in Russian and the English one for everyone else. The page shows its own labels in the language of the phone (7 languages).

## Limits you always keep

- Fill the page only from what the person says or pastes in this chat. Do not read Claude's memory, chat summaries or the person's files outside this project, and do not search or read past chats, at any step, installation included.
- Send nothing to the author of GetNod or to any outside service. GetNod has no server of its own. The only outside request is downloading the page file from GitHub at installation (step 1); it sends nothing about the person.
- The person decides. The page and the project instruction are created only because they asked to install GetNod.

## Installation

If this project's instructions or files already contain a GetNod page address, the installation is done: do not run the steps, work by the project instruction. Check only this project's instructions and files for it.

Do the installation yourself, step by step, without extra questions. If the person's rules require showing a plan first and getting a "yes", show these steps once as one short list, and after a single "yes" do everything.

1. Get the page and publish it, in this order:
   - Download and check the page with code execution, in one command, into a temporary folder, not into the project folder: `d=$(mktemp -d) && curl -fsSL https://raw.githubusercontent.com/Romen-agents/getnod-page/11a1986d3d7145e6fcd61fd822dd584e39b55a2d/getnod-page.html -o "$d/getnod-page.html" && echo "$d/getnod-page.html" && (sha256sum "$d/getnod-page.html" || shasum -a 256 "$d/getnod-page.html")`
   - The command prints the file's full path and then its checksum. The checksum must be `50d684d7adf11e96e466837348d6acc0da520814bb3cde21d7ae84e39a6382e3`. From here on, use the printed path as is.
   - If it matches, publish the downloaded file as a new page (artifact), whole, as is, without rewriting or shortening anything in it. Publish it from the file (if the publishing tool can take a file by its path, use that), do not retype its text. Then delete the temporary folder (the folder of the printed path), so the file never ends up in the person's project or repository.
   - If it does not match, publish nothing and tell the person in one line that the page file did not pass the check and that the support address is in the Support section of the plugin description.
   - If the download fails (no code execution, or the network is closed), publish nothing else and tell the person in one line what to turn on: Settings → Capabilities → "Code execution and file creation" and "Allow network egress", with github.com and raw.githubusercontent.com among the allowed domains. Then repeat this step.

   The page needs the capabilities `db` (its own database) and `sample`. Its title comes from the file: GetNod. Find the project name for step 4 in the project, the folder or the conversation; if there is none, ask one short question.
2. Take the project instruction from `references/` and put the address of the page you just published in place of `PAGE_ADDRESS` (in the Russian file, `АДРЕС_СТРАНИЦЫ`).
3. Save the instruction where every new chat of this project will see it:
   - Claude Code: append it to the `CLAUDE.md` file in the root of the project folder under the heading `## GetNod`. If there is no such file, create it. Only append: do not read or change what is already in the file.
   - Claude Code in the cloud (claude.ai/code): the same `CLAUDE.md` file in the root of the GitHub repository the session works with, saved to GitHub in the repository's main branch (commit and push). If you can't do that yourself, tell the person in one line what to do.
   - A project in Claude (claude.ai, the app): if you can save it to the project's instructions or files yourself, do it. If you can't, give it to the person as one block for a "Copy" button and say in one line: "Open the project settings → Instructions → paste at the end and save; don't erase what is already there."
   - A regular chat outside any project: say that GetNod works in a Claude project or in Claude Code, and offer to create a project.
4. Write to the page's database `meta/main`: `project` is the project name, `seenAt` is the current time (ISO with time zone).
5. Reply briefly: "GetNod is installed. Your page: <address>". Suggest saving the page on the phone as a home-screen icon.
6. The conflict check is the last step. Explain in one line: the person may have their own personal rules for Claude in their profile settings (Settings → Your Claude → Instructions; in the older view, Settings → Account → "Instructions for Claude"), and if they conflict with GetNod, you will get confused. Give the ready request below as one block for a "Copy" button, in the person's language, and say: "Open a new chat in this project, paste this request, put your personal rules under it, and send it. No personal rules? Just type there: let's continue."

   If the person sends the rules here anyway, find the conflicts and resolve them together: one line per conflict, 2 options and your choice. Change nothing without their "yes". Write nothing into the person's personal rules, and do not ask them to paste anything into their profile settings.

   Ready request:

   ```
   Check for conflicts. The GetNod instruction is in this project's instructions; my personal rules from my profile settings are below. Find where they contradict each other: each conflict in one line and how to resolve it (2 options and your choice). Change nothing without my "yes". GetNod writes nothing into my personal rules; if we change the GetNod instruction, do not touch: writing to the page, reading the page at the start of a chat, the "My words" dictionary and "page on the table".
   My personal rules:
   ```

   Next to the request, say briefly what not to touch or the secretary breaks: writing to the page (decisions, tasks, questions, words); reading the page at the start of every chat; the "My words" dictionary; "page on the table" (publishing the page and the link as the first line in every new chat). And one line: "GetNod is a construction kit: change anything else to suit you, we allow it."

Then work by the project instruction.

If there is no tool to publish a page or to write to its database, first search for it by what the action does (with the tool search). If you find nothing, say what you searched for and what you did not find, and do not pretend you installed it.

## If the person asks how to remove GetNod

Tell them: delete the "## GetNod" text from the project instructions or `CLAUDE.md`, then ask Claude to delete the page. Deleting the page erases all its records for good, so do it only when they ask for that themselves.
