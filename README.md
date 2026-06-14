# OpenClaw Reply Threading Skill

`openclaw-reply-threading` is a reusable OpenClaw skill for making message replies behave like lightweight threads.

It teaches an OpenClaw agent to use native reply metadata as a context anchor, so a direct chat can support multiple simultaneous topics without becoming muddled.

## Who This Is For

This skill is for people who use OpenClaw through conversational messaging surfaces such as WhatsApp, Slack, Discord, or similar chat UIs where replies can point at earlier messages.

It is especially useful when:

- You run several topics in one chat.
- You often send short follow-ups like `yes`, `nope`, `?`, or `didn't work`.
- You want the assistant to know which prior message you are responding to.
- You want approvals and corrections scoped to one specific proposal.
- You want less repeated context-setting and fewer confused thread merges.

## What It Changes

Without reply-aware behavior, assistants often treat the newest chronological messages as the whole context. That breaks down when multiple topics are active.

With this skill, the agent follows a clearer model:

1. The user's current message matters most.
2. The message they replied to is the main context anchor.
3. The active topic follows from that anchor.
4. Nearby chronological chat is secondary.

This makes the conversation feel more like natural human chat: reply to the thing you mean, and the assistant follows that thread.

## OpenClaw Fit

OpenClaw often runs inside real messaging channels. Those channels already have useful social structure: replies, quotes, reactions, message IDs, and visible context.

This skill makes OpenClaw use that structure deliberately.

Recommended OpenClaw behavior:

- Preserve incoming reply metadata such as `reply_to_id`, quoted body, and `has_reply_context`.
- Send visible responses using native replies when the transport supports it.
- Treat replies as local thread anchors for reasoning.
- Keep high-impact approvals scoped to the quoted message.
- Fall back to short textual anchors only when native reply metadata is unavailable.

## Installation

Copy the `SKILL.md` file into your OpenClaw or Codex skills directory using the name `openclaw-reply-threading`.

Example layout:

```text
skills/
  openclaw-reply-threading/
    SKILL.md
```

Then restart or reload the agent environment so the skill metadata is indexed.

## Usage Examples

See [`examples/reply-threading.md`](examples/reply-threading.md) for realistic examples.

## Design Principles

- Replies are context, not decoration.
- Short messages should be resolved against their reply target.
- Thread locality beats raw recency.
- Native reply UI should reduce assistant verbosity.
- Public, external, or irreversible actions still need clear approval.

## Repository Keywords

OpenClaw, Codex skill, WhatsApp replies, reply metadata, conversational agents, AI assistant workflow, message threading, multi-topic chat, context anchoring.

## License

MIT

