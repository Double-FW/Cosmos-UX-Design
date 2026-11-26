---
title: Chatbots
description: Help people start, read and control a text conversation and reach another support route.
---

# Chatbots

## Contents

- [Introduction](#introduction)
- [Recommended markup](#recommended-markup)
- [Recommended layout](#recommended-layout)
- [Recommended behaviour](#recommended-behaviour)
- [Reference implementation](#reference-implementation)
- [Testing notes](#testing-notes)
- [Sources](#sources)

## Introduction

Use a chatbot to help people complete a defined task or find support. Tell people they are using an automated service, what it can help with and how to get help another way. Keep the alternative available before and during chat. Someone who cannot use the conversation, or whose question is outside its scope, still needs a route to complete the task. See [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md).

## Recommended markup

- Use a native button to reveal a chat panel, with its expanded state and relationship to the panel exposed. Use a link to open a separate chat page. Give the panel a visible heading. Follow [Disclosure](Disclosure.md#custom-html) for an in-page non-modal panel or [Modal Dialogs](Modal%20Dialogs.md) if opening chat makes the page inactive.
- Present messages in chronological source order under a conversation heading. Identify the sender in text and distinguish messages from status updates. Keep links and controls in replies operable through native semantics, with names that identify their purpose.
- Give the message field a persistent visible label. Use a textarea and an explicit Send button. Let Enter insert a new line in the textarea. Keep optional shortcuts discoverable and compatible with text composition and assistive technology. See [Forms](Forms.md).
- Choose one announcement route for each event. A named `role="log"` can announce newly appended messages in a short sequential chat. For long or streamed replies, consider an ordinary transcript with a separate polite status and a way to reach the latest reply. Follow [Notifications and Status Messages](Notifications%20and%20Status%20Messages.md#persistent-inline-feedback).

## Recommended layout

Keep the conversation, composer, close control and alternative support route easy to find. Put a link to the latest reply near the composer when navigating the history would take time. Provide a route back to the composer when the transcript is long. Keep keyboard and visual order predictable.

At narrow widths and high zoom, use an in-flow layout or a dedicated page if a floating panel would cover focused controls or essential content. Check sticky composers, the on-screen keyboard, visible focus, wrapping of long links and forced colours. If a transcript scrolls separately, keep its beginning and end reachable with the keyboard. See [Layout](Layout.md#visual-order-versus-tab-order) and [Focus](Focus.md#focus-indication).

## Recommended behaviour

### Open, read and close

Open chat on request. For an in-flow disclosure, focus can stay on the opener and proceed into the panel. If a floating panel deliberately moves focus on entry, choose and test a meaningful target. Keep the rest of the page operable in a non-modal design. When closing hides the focused element, return focus to the opener or a useful replacement. Check what happens to drafts and history on close, reopen and expiry, and explain any loss before people rely on the feature.

### Send, wait and recover

Keep focus and reading position stable when a reply arrives. Show when a response is being prepared, ready or interrupted. For streaming, avoid announcing every token. Decide whether to announce complete messages, meaningful chunks or a short completion notice. Test the choice with supported screen readers while someone is reading history or typing. Give people control over when to move to the latest reply.

If a response can be interrupted, offer ‘Stop generating’. Where you retain partial output, identify it and make retry behaviour clear. If sending fails, retain the unsent message where possible and offer retry without repeating accepted messages. Avoid unnecessary time limits and make any expiry, available extension and recovery route clear.

### Provide another route and protect the task

Give concise replies and make next actions explicit. Identify whether a person will take over, when support is available and what information will be shared. Ask before transferring conversation details where handoff needs permission. Keep the alternative usable if chat fails or scripts are blocked. Use [Support, Escalation and Handoffs](Support%2C%20Escalation%20and%20Handoffs.md#carry-the-task-across-a-handoff) for the support journey. If the chatbot can perform consequential actions, show the details for review and obtain confirmation before acting.

## Reference implementation

This standalone fixture demonstrates an in-flow panel, labelled composer, controlled announcement, stop action and latest-reply link. Copy it into an HTML file to try it. It generates a fixed local reply and keeps messages and drafts in memory until reload. The support link points to a local example section. Replace that section with working support for a real service. Use the production checks below for transport failures, streaming and handoff.

```html
<!doctype html>
<html lang="en-GB">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Chat accessibility example</title>

<style>
  #chat-panel { max-inline-size: 42rem; overflow-wrap: anywhere; }
  #chat-panel textarea { inline-size: 100%; box-sizing: border-box; }
  #chat-panel :focus-visible { outline: 2px solid currentColor; outline-offset: 2px; }
  #chat-panel [aria-disabled="true"] { border-style: dashed; }
</style>

<main>
  <h1>Booking support</h1>
  <p><a href="#other-support">View other ways to get help</a></p>
  <button type="button" id="chat-open" aria-controls="chat-panel"
    aria-expanded="false" hidden>Open booking chat</button>

  <section id="chat-panel" aria-labelledby="chat-title" hidden>
    <h2 id="chat-title">Booking chat</h2>
    <p>Automated assistant for booking questions. This example uses fixed replies.</p>
    <button type="button" id="chat-close">Close booking chat</button>
    <h3 id="history" tabindex="-1">Conversation</h3>
    <ol id="chat-history"></ol>
    <p><a href="#history">Read conversation from the start</a></p>
    <p><a id="chat-latest" hidden>Read latest reply</a></p>
    <form id="chat-form">
      <label for="chat-message">Your message</label>
      <textarea id="chat-message" rows="3" required></textarea>
      <button type="submit" id="chat-send">Send message</button>
      <button type="button" id="chat-stop" aria-disabled="true">Stop generating</button>
    </form>
    <p id="chat-status" role="status"></p>
  </section>

  <section id="other-support" tabindex="-1">
    <h2>Other ways to get help</h2>
    <p>Example support route. In a live service, provide working contact options here.</p>
  </section>
</main>

<script>
  const byId = id => document.getElementById(id);
  const opener = byId('chat-open');
  const panel = byId('chat-panel');
  const form = byId('chat-form');
  const message = byId('chat-message');
  const send = byId('chat-send');
  const stop = byId('chat-stop');
  const status = byId('chat-status');
  const latest = byId('chat-latest');
  let pending = null;
  let messageNumber = 0;
  let replyNumber = 0;

  function addMessage(sender, content, replyId) {
    const item = document.createElement('li');
    const heading = document.createElement('h4');
    heading.textContent = sender + ', message ' + (++messageNumber);
    if (replyId) {
      heading.id = replyId;
      heading.tabIndex = -1;
    }
    const body = document.createElement('p');
    body.textContent = content;
    item.append(heading, body);
    if (replyId) {
      const back = document.createElement('a');
      back.href = '#chat-message';
      back.textContent = 'Back to your message';
      item.append(back);
    }
    byId('chat-history').append(item);
  }

  function setBusy(busy, feedback) {
    send.setAttribute('aria-disabled', String(busy));
    stop.setAttribute('aria-disabled', String(!busy));
    status.textContent = feedback;
  }

  opener.addEventListener('click', () => {
    panel.hidden = !panel.hidden;
    opener.setAttribute('aria-expanded', String(!panel.hidden));
  });

  byId('chat-close').addEventListener('click', () => {
    opener.focus();
    panel.hidden = true;
    opener.setAttribute('aria-expanded', 'false');
  });

  form.addEventListener('submit', event => {
    event.preventDefault();
    if (pending !== null) return;
    const text = message.value.trim();
    if (!text) {
      status.textContent = 'Enter a message to send.';
      return;
    }
    addMessage('You', text);
    message.value = '';
    setBusy(true, 'Generating reply.');
    pending = window.setTimeout(() => {
      pending = null;
      const replyId = 'chat-reply-' + (++replyNumber);
      addMessage('Automated assistant',
        'You can open your saved booking to check its collection time.', replyId);
      latest.href = '#' + replyId;
      latest.hidden = false;
      setBusy(false, 'Reply ready. Use Read latest reply to find it.');
    }, 1200);
  });

  stop.addEventListener('click', () => {
    if (pending === null) return;
    window.clearTimeout(pending);
    pending = null;
    setBusy(false, 'Generation stopped. Your conversation is still available.');
  });

  opener.hidden = false;
</script>
```

The example announces status through a single region and leaves the transcript available through normal navigation. Render plain message text as text. If your product supports formatted replies, audit its renderer and let people use headings, lists, links and controls with the same semantics as on the rest of the site.

## Testing notes

Run these checks on the integrated host page. Record the supported browser, device, assistive technology and observed result. Use fictitious conversations and controlled responses to exercise delays and failures. Inspect names, relationships, hidden states, focus and retry behaviour with automated checks where possible. Observe actual announcements and usability manually.

| Starting state and action | Expected result |
| --- | --- |
| Start with chat closed. Use Tab and a supported screen reader to find, open, navigate and close chat. In a separate run, block scripts. | The launcher identifies chat and exposes its state. Focus is usable after closing. Hidden controls are skipped and the alternative support route is available. |
| Type multiple lines and send using the named button. Test any optional keyboard shortcut with input-method composition. | Enter allows a line break in the textarea and the message is sent only through the intended action. The composer has a clear name and remains usable. |
| Send a message, begin a new draft or read earlier messages, then let the reply arrive. Repeat with streamed responses and rapid updates where offered. | The draft and reading location remain usable. Feedback identifies progress and completion without repeating the transcript or individual tokens. The latest reply and older messages can be reached on demand. |
| Stop a pending response, retry after a failed request, and close and reopen with a draft. | Stop prevents further output for the cancelled response. Partial output is identified if retained. A retry does not duplicate an accepted message, and any draft loss is explained. |
| Increase text size, then separately test a narrow viewport, forced colours and the touch keyboard. If the host uses a modal, test focus containment and return. | Chat and its support route stay operable. The composer and chat panel do not hide focused controls. The modal variant follows the [Modal Dialogs](Modal%20Dialogs.md) checks. |
| Ask a question outside the tool’s scope and follow the human support or alternative route. If handoff is offered, try accepting and declining the transfer. | The service explains the route and any wait or data transfer. People can continue safely in either case. |

Use the [Accessible Chatbot Testing Brief](https://github.com/Double-FW/Accessible-Chatbots/blob/c6da49d3bb35d51393898a5c117aa89cc5d6ed63/Accessible_Chatbot_Testing_Brief.md) for a fuller test matrix, including long histories, embeds, reauthentication, attachments and voice if supplied.

## Sources

- [Double-FW Inclusive Chatbot Interface Reference](https://github.com/Double-FW/Accessible-Chatbots/blob/c6da49d3bb35d51393898a5c117aa89cc5d6ed63/Inclusive_Chatbot_Interface_Reference.md) and [Accessible Chatbot Testing Brief](https://github.com/Double-FW/Accessible-Chatbots/blob/c6da49d3bb35d51393898a5c117aa89cc5d6ed63/Accessible_Chatbot_Testing_Brief.md)
- [W3C technique ARIA23: Sequential information updates](https://www.w3.org/WAI/WCAG22/Techniques/aria/ARIA23)
- [W3C Understanding Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html)
- [W3C ARIA Authoring Practices Guide: Modal Dialog](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)
- [GOV.UK Service Manual: Using chatbots and webchat tools](https://www.gov.uk/guidance/using-chatbots-and-webchat-tools)
