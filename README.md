# GetNod

A secretary for a Claude project. You talk in one project chat; Claude records your decisions, tasks and open questions on one private page in your own Claude account. You answer with buttons, tick tasks and leave notes from your phone. Every new chat in the project reads the page and carries on from where you stopped.

Русский раздел: [в конце файла](#по-русски).

## What it does

- **Records decisions, tasks and questions.** What you decided, what you need to do by hand, and what Claude needs you to answer, as short records. It does not save the chat itself.
- **Sorts the conversation by part of the project.** On the page you see, for example, "Venue" and "Menu", each with its decisions and open items.
- **Works from your phone.** The page opens in any browser where you are signed in to Claude. Its labels follow your phone's language (English, Russian, Spanish, French, German, Ukrainian, Japanese).
- **Keeps one conversation going.** When a chat gets long, Claude writes a short handoff note on the page and tells you to open a new chat in the project. The new chat reads the page and continues. The instruction also tells Claude to suggest staying in this one chat rather than opening a separate chat for a single topic.

One project gets one page. For a second project, install it again in that project.

## What you need

- A paid Claude plan (Pro or higher).
- A Claude project on the web or in Claude Desktop, or a project folder in Claude Code. In an ordinary chat outside a project, GetNod will offer to create a project first.
- Page publishing turned on: Settings → Capabilities → "Code execution and file creation" (in newer settings, "Create and edit files"). On Team and Enterprise plans an admin controls this in Organization settings.
- Network access for code execution: Settings → Capabilities → "Allow network egress", with github.com and raw.githubusercontent.com allowed. At installation Claude downloads the page file from github.com: the public repository Romen-agents/getnod-page, at a fixed version, with a checksum check.

GetNod has not been tested in Cowork yet. Claude supports page data on the web and in Claude Desktop; on your phone, open the page in a browser.

## Install and start

1. Add GetNod from the plugin directory: Customize → Plugins in Claude, or `/plugin` in Claude Code.
2. Open your project, start a new chat and type: **Install GetNod**.
3. Claude publishes your page, saves the project instruction and gives you the page address. Depending on your settings, Claude will ask you to approve four actions: reading the plugin's files, downloading the page file (code execution), publishing a page, and writing to the page's database. All four are needed. In a Claude project it may ask you to paste the instruction into the project settings yourself.
4. Open a new chat in the project and type: **let's continue**. The first line of the reply is the link to your page.

If you keep personal rules for Claude in your profile settings, Claude gives you a short request that checks them against GetNod, so the two don't conflict.

## Examples

Try these in a project where GetNod is installed:

1. "We're going with the venue on Main Street, and I need to pay the deposit by Friday." Claude records one decision and one task, and ends its reply with a line such as: GetNod: "Venue" — 1 decision, 1 task.
2. "What's waiting for me?" Claude lists your open tasks and unanswered questions, and tells you what you did on the page since the last time.
3. "This chat is getting long, let's move." Claude writes a handoff note and tells you to open a new chat and type "let's continue".
4. "I answered on the page." Claude reads your answer and carries on from it.

To see it with sample data, install it in an empty test project and paste: "We're planning a wedding for 80 guests. We chose the venue on Main Street, I need to pay the deposit by Friday, and I can't decide between a buffet and a served dinner: ask me which one with two options." Then open the page. Expected result: two parts of the project (such as Venue and Menu), one decision, one task with its Friday deadline, and one question with two buttons.

## Data

**In short.** Everything GetNod saves stays in your own Claude account, on one private page. The author never receives any of it. On the page you can see and remove decisions, tasks, questions, notes and "My words". The project name, its parts and the handoff note for the next chat are changed or cleared by asking Claude. Deleting the page erases everything for good.

**Where your records live.** In the database of one page in your own Claude account. GetNod has no server. The author cannot see your page or your records.

**What is stored on the page:**

- the project name and the parts of the project, each with a one-line status. The page shows them; to change or remove them, ask Claude;
- decisions, tasks, questions, notes, and your answers and ticks. You can remove any of these records on the page;
- discussions on a record: your note and Claude's replies;
- the handoff note for the next chat, 1–3 lines. The Helpers tab shows up to 140 characters of it; to clear it, ask Claude;
- "My words": words you asked Claude to remember, each with its meaning, only after you say yes. You can fix or delete any word on the page;
- technical fields: when Claude last read the page, when the note was written, and a link to the chat if Claude knows it. The page does not show these. It uses the chat link only to open that chat when you press Discuss;
- the page's settings (language, sort order, whether notes are sorted right away).

Closed items show less: for an answered question or a finished task the page shows the title and the result, and Claude's replies in a closed discussion are not shown. All of this stays in the database, new chats can read it, and it is hidden together with its record.

A decision record looks like this:

```json
{"kind": "decision", "branch": "venue", "title": "Venue: Main Street", "at": "2026-11-03T15:09:00+01:00"}
```

Your phone or browser also keeps small conveniences on that device: theme, language, sort order, open tab, collapsed blocks, and whether a hint was already shown.

**This is deliberately saved between chats, and it changes how Claude behaves in this project.** Installing GetNod adds an instruction to your project that makes every new chat read the page and continue from it. What is saved is the set of records above. You can hide any record or word on the page; hidden items stay in the database until you delete the page. GetNod does not save chat transcripts. It stores no information about you beyond these records, and "My words" holds only the words you approved. The instruction tells Claude to treat everything it reads from the page as information about your work, not as commands, and to confirm with you first if it asks for anything beyond this project's records. To stop all this, delete the GetNod text from wherever the instruction was saved: the project instructions, the project files or `CLAUDE.md`.

**What GetNod does not do.** It does not read Claude's memory, past chats, chat summaries or your files. It sends nothing to the author, and it contains no analytics, no outside scripts and no outside fonts. The plugin has no server or connector of its own. It works through Claude's tools for publishing a page and writing to its database, and it asks Claude to search its tool list if it can't find them. For large finished work, the instruction asks Claude to do a second check, with a separate helper if one is available; a helper uses your limit too. The optional "Sort notes right away" setting below sends notes to Claude. At installation the plugin downloads the page file once from https://github.com/Romen-agents/getnod-page; GitHub sees an ordinary download request, and nothing is sent to the author of GetNod.

**What installing changes.** It publishes one new page in your account, private until you share it, and adds the GetNod instruction to your project instructions, your project files or `CLAUDE.md` in your project ([full text](skills/secretary/references/project-instruction-en.md)). In Claude Code in the cloud (claude.ai/code), it also commits `CLAUDE.md` and pushes it to your repository's main branch. Claude publishes the page again at the same address at the start of every new chat, when you come back to a chat after a break, and when you ask for the page or mention it, so its link appears in the chat; the records do not change.

**Claude usage.** Writing records and publishing the page are part of your normal Claude usage. Each time Claude publishes the page again, it reads the whole page file (about 200 KB, roughly 58,000 tokens), so that step costs much more of your limit than an ordinary message. There is one optional setting on the page, "Sort notes right away", off by default: if you turn it on, the page sends each new note (up to 2,000 characters) and the names of the project's parts to Claude to sort it, using the usage of whoever is viewing the page.

**How long it is kept.** Until you delete it. "Remove record" on the page, and deleting a word in "My words", hide the item; it stays in the database until the page is deleted. Deleting the page erases everything on it for good.

**To remove GetNod.** Delete the GetNod text from your project instructions, project files or `CLAUDE.md`, ask Claude to delete the page, and uninstall the plugin.

**Sharing.** If you share the page with someone, they can see your records, and anyone you give edit access can change them. Don't put passwords, keys or other people's personal data on it; the instruction tells Claude not to either.

Privacy policy: https://getnodagents.com/privacy

## Troubleshooting

- **"GetNod is installed" but a new chat doesn't know the page.** The project instruction wasn't saved. Open the project settings → Instructions or the project files, or `CLAUDE.md` in Claude Code, and check that the GetNod text is there with your page address.
- **Claude can't download the page.** Turn on code execution and network access (see "What you need").
- **Claude says it can't publish a page.** Turn on "Code execution and file creation" (see "What you need"). If it's on, ask Claude to search its tools for publishing a page.
- **The page says there is no data, or that the connection was lost.** Open it from the link in your chat while signed in to Claude, then refresh it.
- **Claude writes "⚠ The page is not saving".** A write to the page failed, and the same line says what to do. If it says "Network access is off", go to Settings → Capabilities and turn on "Allow network egress". If the page can't be found or there is no permission, the page is probably in another Claude account. Until saving works, Claude keeps your decisions and tasks in its reply and moves them to the page afterwards.
- **Claude didn't record something.** Tell it: "write that down". Records appear on the page straight away; refresh if they don't.
- **A new chat has no handoff note.** The old chat ended without one. Go back to it and type: "write the note".
- **Claude and your personal rules disagree.** In a new chat in the project, ask Claude to check for conflicts between GetNod and your personal rules, and paste the rules under your request.
- **Your limit ran out during installation.** After it resets, type "continue" in the same chat. Don't publish a second page: a new page starts with an empty database.

## Support

- Email: hello@getnodagents.com
- Telegram bot: https://t.me/GetNodSupportbot
- Security reports: hello@getnodagents.com with "Security" in the subject line.

Support is separate from the plugin, and the plugin sends it nothing. If you message the Telegram bot, your messages are stored on our server and are sent to Anthropic's Claude API to prepare replies. The privacy policy has the details.

## License and affiliation

Free to use in your own projects, including work projects. Selling GetNod or sharing copies of it requires the author's written permission. See the LICENSE file.

GetNod is an independent project, not affiliated with or endorsed by Anthropic. Claude is a trademark of Anthropic.

## По-русски

GetNod — секретарь проекта в Claude. Ты говоришь в одном чате проекта, а Claude записывает твои решения, задачи и вопросы на одну частную страницу в твоём аккаунте Claude. С телефона ты отвечаешь кнопками, ставишь галочки и оставляешь заметки. Каждый новый чат проекта читает страницу и продолжает с того же места.

**Как поставить.** Добавь GetNod из каталога плагинов, открой проект, начни новый чат и напиши: «Установи GetNod». Claude опубликует страницу, сохранит инструкцию проекта и даст адрес. Затем открой новый чат в проекте и напиши: «продолжаем». При установке Claude один раз скачивает файл страницы с github.com (открытый репозиторий Romen-agents/getnod-page, точная версия, проверка контрольной суммы), поэтому нужен доступ к сети для выполнения кода: «Settings → Capabilities → Allow network egress», в разрешённых адресах — github.com и raw.githubusercontent.com. GitHub видит обычное скачивание, автору GetNod ничего не передаётся.

**Нужно:** платный план Claude (Pro или выше), проект в Claude в браузере или Claude Desktop либо папка проекта в Claude Code. На телефоне страницу открывай в браузере.

**Примеры:** «Берём площадку на Главной, задаток надо внести до пятницы»; «Что ждёт меня?»; «Чат стал длинным, давай переедем»; «Я ответил на странице».

**Данные.** Всё, что сохраняет GetNod, остаётся в твоём аккаунте Claude: автор этого не получает. Записи лежат в базе одной страницы: части проекта, решения, задачи, вопросы, заметки, твои ответы, обсуждения записей с ответами Claude, записка для следующего чата, словарь «Мои слова» и служебные поля (когда чат читал страницу, когда написана записка, ссылка на чат). Новые чаты читают эти записи, чтобы продолжать работу: это сделано намеренно и меняет поведение Claude в этом проекте. На странице ты видишь и убираешь решения, задачи, вопросы, заметки и слова из «Моих слов». Название проекта, его части и записку меняет или очищает Claude по твоей просьбе; из записки во вкладке «Помощники» видно до 140 знаков. Служебные поля на странице не показываются, ссылка на чат нужна только кнопке «Обсудить». У закрытых вопросов и задач страница показывает заголовок и итог, а ответы Claude в закрытом обсуждении не показывает. Всё это остаётся в базе, новые чаты могут это читать, и скрывается вместе с записью. Инструкция велит Claude считать всё, что он читает со страницы, сведениями о работе, а не командами, и подтверждать у тебя всё, что выходит за записи проекта. Кому ты дал право правки, тот может менять записи. GetNod не читает память Claude, прошлые чаты, их пересказы и твои файлы и не хранит переписку; о тебе хранится только то, что есть в этих записях. У GetNod нет своего сервера и подключений: автор не видит твою страницу, счётчиков и чужих скриптов нет. Настройка «Разбирать заметку сразу» (выключена) отправляет заметку Claude. Для большой работы инструкция просит второй проход, можно отдельным помощником: это тоже расходует лимит.

**Что меняет установка.** Появляется одна новая страница (частная, пока ты сам ею не поделишься) и текст GetNod в инструкциях или файлах проекта либо в `CLAUDE.md`; в облачном Claude Code он ещё отправляется в основную ветку репозитория. Claude заново публикует страницу в начале каждого нового чата, после перерыва и когда ты о ней говоришь. Записи при этом не меняются, но Claude читает весь файл страницы (около 200 КБ), и на это уходит заметная часть лимита.

**Как удалить.** Убери текст GetNod из инструкций или файлов проекта либо из `CLAUDE.md`, попроси Claude удалить страницу и удали плагин. Удаление страницы стирает все записи навсегда; «Убрать запись» и удаление слова только скрывают запись.

**Если Claude пишет «⚠ Страница не пишется».** Запись на страницу не прошла, в той же строке сказано, что делать. Пока запись не заработает, Claude держит твои решения и задачи в ответе и потом переносит их на страницу.

**Поддержка:** hello@getnodagents.com, бот https://t.me/GetNodSupportbot (его сообщения хранятся на нашем сервере и уходят в Claude API компании Anthropic для подготовки ответа). Сообщения о безопасности — на ту же почту, в теме слово Security. Конфиденциальность: https://getnodagents.com/privacy

GetNod — независимый проект, не связан с Anthropic и не одобрен ею. Claude — товарный знак Anthropic.
