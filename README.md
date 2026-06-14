# OpenClaw Reply Threading Skill

`openclaw-reply-threading` is a reusable OpenClaw skill for making message replies and emoji reactions behave like lightweight threads.

It teaches an OpenClaw agent to use native reply and reaction metadata as context anchors, so a direct chat can support multiple simultaneous topics without becoming muddled.

## Who This Is For

This skill is for people who use OpenClaw through conversational messaging surfaces such as WhatsApp, Slack, Discord, or similar chat UIs where replies and reactions can point at earlier messages.

It is especially useful when:

- You run several topics in one chat.
- You often send short follow-ups like `yes`, `nope`, `?`, or `didn't work`.
- You want the assistant to know which prior message you are responding to.
- You use emoji reactions as lightweight acknowledgments, approvals, or priority signals.
- You want approvals and corrections scoped to one specific proposal.
- You want less repeated context-setting and fewer confused thread merges.

## What It Changes

Without reply-aware behavior, assistants often treat the newest chronological messages as the whole context. That breaks down when multiple topics are active.

With this skill, the agent follows a clearer model:

1. The user's current message matters most.
2. The message they replied to is the main context anchor.
3. A reaction is a compact signal scoped to the reacted-to message.
4. The active topic follows from that anchor.
5. Nearby chronological chat is secondary.

This makes the conversation feel more like natural human chat: reply to the thing you mean, and the assistant follows that thread.

## Emoji Reactions As Replies

Emoji reactions are not full messages, but they are still attached to a specific message. This skill treats them as reaction-scoped context anchors.

That means a `👍`, `✅`, `👀`, `❤️`, `😂`, `🤔`, or `❌` should be interpreted against the message it reacted to, not as a loose signal about the whole conversation.

Default interpretation:

- `👍`, `✅`, `🙌`: acknowledgment or approval of the reacted-to message.
- `👀`: seen, tracking, or worth watching.
- `❤️`: strong appreciation or personal resonance.
- `😂`, `💀`: amusement; not task approval.
- `🤔`: uncertainty, curiosity, or needs more thought.
- `❌`, `👎`: rejection or disagreement.

Reaction-only events usually should not trigger a chat response. They should quietly update the agent's working context, task state, or priority model unless a follow-up is clearly useful.

Approval rules stay conservative:

- Low-risk internal actions can use a clear positive reaction as approval when the reacted-to message contains the action.
- Public, external, costly, or irreversible actions still need explicit text confirmation unless the workflow already established that a reaction counts.
- A reaction only applies to the message it is attached to. It does not approve adjacent plans or later actions.

## OpenClaw Fit

OpenClaw often runs inside real messaging channels. Those channels already have useful social structure: replies, quotes, reactions, message IDs, and visible context.

This skill makes OpenClaw use that structure deliberately.

Recommended OpenClaw behavior:

- Preserve incoming reply metadata such as `reply_to_id`, quoted body, and `has_reply_context`.
- Preserve reaction metadata such as emoji, reacted-to message ID, and target body when available.
- Send visible responses using native replies when the transport supports it.
- Treat replies and reactions as local thread anchors for reasoning.
- Keep high-impact approvals scoped to the quoted message.
- Treat reaction-only approvals conservatively for public or irreversible actions.
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
- Reactions are scoped signals, not decoration.
- Short messages should be resolved against their reply target.
- Thread locality beats raw recency.
- Native reply UI should reduce assistant verbosity.
- Public, external, or irreversible actions still need clear approval.

## Repository Keywords

OpenClaw, Codex skill, WhatsApp replies, emoji reactions, reply metadata, reaction metadata, conversational agents, AI assistant workflow, message threading, multi-topic chat, context anchoring.

## License

MIT
