---
name: openclaw-reply-threading
description: Use replies and reactions as context anchors in OpenClaw chats so multi-topic work stays fast, organized, and natural.
---

# OpenClaw Reply Threading

Use native message replies and reactions as lightweight thread anchors. This lets a user and OpenClaw keep several topics moving in one chat without repeatedly restating context.

The core behavior: when a user message includes reply metadata, treat the replied-to message as the primary local context for the new request.

When a user reacts with an emoji, treat the reaction as a compact signal scoped to the reacted-to message. A reaction is weaker than a text reply, but it is still meaningful context.

## Context Priority

Use this order:

1. Current user text.
2. Replied-to message body and metadata.
3. The active thread implied by that replied-to message.
4. Recent surrounding conversation.
5. Long-term memory or external tools, only when relevant.

If the reply target conflicts with nearby chronological context, prefer the reply target. Chronology can help, but the reply anchor defines the active thread.

## What Replies Mean

A reply can signal:

- Continuation: the user is continuing the quoted topic.
- Correction: the user is correcting or rejecting the quoted answer.
- Selection: the user is choosing one item from a numbered or bulleted answer.
- Approval: the user is approving the specific quoted proposal.
- Fork: the user is starting a subtopic from one prior message.
- Test: the user is checking reply plumbing.

Short messages like `yes`, `nope`, `?`, `do it`, `same`, or `didn't work` should be interpreted against the quoted message before asking a follow-up question.

## What Reactions Mean

A reaction can signal:

- Acknowledgment: the user saw or accepts the message.
- Approval: the user likes or agrees with the specific message.
- Disagreement: the user rejects or questions the specific message.
- Priority: the user is marking the message as important.
- Completion: the user is indicating the item is done.
- Interest: the user wants more on that topic later.

Interpret reactions conservatively. They are excellent for lightweight organization, but they usually should not trigger large actions by themselves.

Common defaults:

- `👍`, `✅`, `🙌`: positive acknowledgment or approval of the reacted-to message.
- `👀`: seen, tracking, or worth watching.
- `❤️`: strong appreciation or personal resonance.
- `😂`, `💀`: humor or amusement; do not treat as task approval.
- `🤔`: uncertainty, curiosity, or needs more thought.
- `❌`, `👎`: rejection or disagreement with the reacted-to message.

These meanings are defaults, not universal rules. Prefer the user's established style when known.

## Response Behavior

When sending a visible reply on a surface that supports native replies, reply to the inbound message that triggered the response.

For WhatsApp through OpenClaw, if the messaging tool supports `replyTo`, pass the current inbound `message_id` as `replyTo`.

Use a light textual anchor only when it adds clarity:

- `Re Bee: ...`
- `On the quote test: ...`
- `For the voice script: ...`

Do not over-label routine messages. The native reply UI should carry most of the organization.

For reaction-only events, usually do not send a message. Update local interpretation of the thread, task, or priority quietly unless the reaction clearly requires a follow-up.

## Multi-Topic Workflow

When several topics are active:

- Treat each reply chain as its own working thread.
- Answer the thread the user replied to unless they explicitly ask for a roundup.
- Preserve decisions, assumptions, and next steps independently per thread.
- If a new message is not a reply and spans several topics, respond with compact topic-labeled bullets.
- If the user replies to a list item, infer the selected item from the quoted content and current text.

This makes one chat behave more like several clean, low-friction threads.

## Ambiguity Handling

Ask only when the reply anchor is missing or still does not disambiguate the request.

Useful fallback prompts:

- `I see this message, but no reply anchor came through. Which topic is this for?`
- `I can see the reply target, but there are two plausible asks in it. Do you mean the Bee setup or the quote-test patch?`

If one interpretation is clearly more likely, proceed and state the assumption briefly.

## Action Safety

A reply can approve only the action described in the quoted message.

An emoji reaction can support low-risk approval or acknowledgment, but it should not be the only basis for external, public, costly, or irreversible actions unless the surrounding workflow explicitly established that reaction as approval.

Before external, public, costly, or irreversible actions:

- Verify that the quoted message contained the proposed action.
- Verify that the current user text or established reaction workflow clearly approves that action.
- If reply or reaction metadata is missing, do not infer high-impact approval from a bare `yes` or a detached emoji.

Examples:

- If the user replies `yes` to a draft-send proposal, that can approve sending that specific draft.
- If the user replies `yes` to a general discussion message, that is not enough approval for a new external action.
- If the user replies `do it` to a public publishing plan, keep the action scoped to the quoted plan.
- If the user reacts `👍` to a low-risk internal organization proposal, that can be treated as approval.
- If the user reacts `👍` to a public-send or publish proposal, ask for text confirmation unless the workflow already says that reaction is sufficient.

## Memory And Follow-Through

Use the right durable system for the outcome:

- Use Skill Workshop for reusable procedures or standing workflows.
- Use workspace memory for stable facts or preferences worth remembering.
- Use daily notes for raw session events only when requested or useful.

Do not promote every thread detail into long-term memory. Reply metadata and recent context are usually enough.

## Debugging Reply Support

Verify inbound and outbound behavior separately:

- Outbound: assistant messages render as native replies in the user's app.
- Inbound: user replies arrive with a reply target such as `has_reply_context`, `reply_to_id`, and a quoted target body or retrievable target ID.
- Reactions: user reactions arrive with the emoji, reacted-to message ID, and target message body or retrievable target ID.
- Reasoning: the assistant actually uses the quote target to resolve short or ambiguous messages.

If outbound native replies fail visually, use a short textual anchor while debugging transport.

If inbound metadata is missing, ask for a short topic label and continue. Do not pretend the metadata was available.
