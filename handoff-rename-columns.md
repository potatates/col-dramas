# Handoff: rename tabs and sections on DramaCrave + FeverDrama

## Goal
DramaCrave and FeverDrama (short-drama H5 sites) still use the tab and section names copied from ReelsWave, including emojis. Change the **display names only** (`showName`) to the names below. Do NOT change `columnName`, banners, content, order, page bindings or anything on other clients.

## How
Use the skill `rocnovel-client-columns` (zip from 庄钰莹: `rocnovel-client-columns.zip`; read SKILL.md and references/api.md first).
- API base: `https://admin-api.rocnovel.com`. Headers: `Authorization`, plus `Origin: https://platform.rocnovel.com` and `Referer: https://platform.rocnovel.com/`.
- Auth: I'll provide the token as the env var `ROCNOVEL_AUTH`, taken from a cURL copied out of the logged-in admin in Chrome. Never print it, log it or write it to a file. If it's missing, ask me to set it. Don't pull it from chat history.
- Content type for short dramas: `contentType = 2`.
- If the API route doesn't work, do it in Chrome instead (platform.rocnovel.com, already logged in): 客户端页面 for tabs, 客户端栏目 for sections → 编辑 → display name.

## Steps
1. `POST /client/group/list` → find the client IDs for **DramaCrave** and **FeverDrama**.
2. For each client: `GET /content/clientPage/pageSearch` and `GET /content/clientColumn/pageSearch` (page through everything). Save the results as `before.json`.
3. Match rows by their current `showName` (see table). If a name matches more than one row, or doesn't match any, stop and ask me.
4. Update each row with `POST /content/clientPage/addAndUpdate` or `/content/clientColumn/addAndUpdate`. Send the **full existing record** (keep the target `id`), with only `showName` changed. Strip derived/audit/`*Str` fields per api.md. One request at a time. Don't retry blindly: re-read the row after any error.
5. Re-read both lists and save them as `after.json`. Check that only `showName` changed on the 7 rows per client.
6. Open https://drama.dramacrave.top/?tabId=453558397453246464 and https://drama.feverdrama.top/?tabId=453141910318002176 and check that the new names show with no emojis. FeverDrama shows them in uppercase via CSS, so store them in normal case. The sites may cache for a while; if the names haven't updated, say so instead of retrying.

## Rename table (exact text)
| Current showName | DramaCrave → | FeverDrama → |
|---|---|---|
| Popular (tab/page) | Most craved | Hot now |
| New (tab/page) | Fresh drops | Just in |
| Category (tab/page) | Genres | Browse |
| 💎CEO & Billionaire (section) | Billionaire obsessions | Billionaire hearts |
| 🔥Revenge & Comeback (section) | They'll regret it | Payback stories |
| 🌙Fantasy & Mystery (section) | Moonlit desires | Wolves & magic |
| 💕Love & Family (section) | Hearts & heartbreak | Love & family ties |

(The current names may differ slightly in emoji or spacing; match on the words.)

## Out of scope (don't do)
- ReelsWave: 庄钰莹 also asked for it, but whether to rename it or only reorder its content is still open. Ask me.
- Putting 自制剧 (in-house dramas) first in sections: wait until we know how 自制剧 are marked (a field or the uploader).
- Banners, subscriptions, payments.

## Report back
For each client: which rows changed (old → new), the IDs, any rows skipped, and whether the live site shows the new names.
