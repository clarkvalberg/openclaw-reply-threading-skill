# Reply Threading Examples

## Short Correction

Assistant:

> I recommend using Bee CLI and MCP.

User replies to that exact message:

> Nope

Expected behavior:

- Interpret `Nope` as a rejection of the Bee CLI recommendation.
- Do not treat it as a global rejection of every active topic.
- Reassess the specific recommendation and ask or research from a corrected premise.

## Approval Scoped To One Action

Assistant:

> I can publish this skill as a public GitHub repository named `openclaw-reply-threading-skill`.

User replies:

> Do it

Expected behavior:

- Treat this as approval for that specific publishing action.
- Do not infer approval for unrelated public actions.

## Parallel Topics

Active topics:

- Debugging WhatsApp quoted replies.
- Creating a voice sample.
- Researching Bee data access.

User replies to the quote-test message:

> Didn't work

Expected behavior:

- Continue the quote-test thread.
- Do not answer about Bee or the voice sample.
- Use diagnostic context from the quoted message.

## Missing Reply Metadata

User says:

> yes

No reply metadata arrives.

Expected behavior:

- If the action is low impact and obvious, proceed with a brief assumption.
- If the action is external, public, costly, or irreversible, ask what the `yes` refers to.

## Reaction As A Scoped Signal

Assistant:

> I can add a recurring private Bee sync every 30 minutes.

User reacts to that exact message with:

> 👍

Expected behavior:

- Treat the reaction as lightweight approval for the specific low-risk internal setup.
- Keep the approval scoped to that message.
- Do not apply the same approval to unrelated setup work.
- A short confirmation is optional; it is usually better to do the work and report only if something changes.

## Reaction-Only Acknowledgment

Assistant:

> I found the failing check. It is a lint error in `src/messages.ts`.

User reacts with:

> 👀

Expected behavior:

- Treat the reaction as "seen" or "tracking."
- Do not send a standalone "got it" reply.
- Keep the finding available as the active context if the user later replies with `fix it`.

## Reaction As Disagreement

Assistant:

> I think we should make replies mandatory for every follow-up.

User reacts with:

> ❌

Expected behavior:

- Treat the reaction as rejection of that specific recommendation.
- Do not treat it as rejection of the whole project.
- Revise the recommendation toward a lighter default, such as replies for ambiguous or parallel topics.

## Reaction Is Not Enough For High-Impact Work

Assistant:

> I can publish this private draft publicly.

User reacts with:

> 👍

Expected behavior:

- Treat the reaction as positive signal, not final authorization.
- Ask for explicit text confirmation before publishing.
- Keep the confirmation prompt scoped to the reacted-to publication proposal.
