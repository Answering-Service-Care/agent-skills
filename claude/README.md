# Answering Service Care

Work with Answering Service Care from Claude.

This plugin connects Claude to the Answering Service Care client connector and adds skills
for the everyday work our clients do: reading and acting on the messages we took for you,
keeping your on-call schedule right, and keeping your on-call people reachable.

## Install

Install from the Claude Directory once published. For local testing, clone this repository and run `claude --plugin-dir claude`.

On first use you sign in at the Answering Service Care client portal with your own login.
Your password is never seen by Claude or by the connector. You see and change only what
your login can in the client portal: the accounts and lines you belong to, and the
calendar of lines where you have Calendar Management access.

## Customer capabilities

The customer connector supports authorized message review and updates,
follow-up to-dos, and on-call calendar and escalation-level management.
Access depends on the signed-in user's active memberships, granted OAuth
scopes, and Calendar Management permission for the affected phone.
Before changes that send calls, SMS or email or change coverage, ask for
confirmation and verify the tool result.

**ASC billing:** A customer whose account grants Billing access can review
a limited balance and get an authenticated link to the secure ASC customer
payment portal. Claude does not take payment, card details or bank details.

## Skills

| Skill | What it does |
|---|---|
| `answering-service-care` | Picks the right skill for a request, and the order when a job needs several |
| `message-triage` | Separate the messages that need action now from the ones that can wait, and record what you did |
| `oncall-changes` | Cover time off, swap or move shifts, and answer who's on call |
| `contact-delivery` | Update how on-call people are reached: numbers, texts, email |
| `billing-handoff` | Check an authorized ASC balance and open its secure payment portal |

The skills only guide the order of the work. Every rule is enforced by the connector, which
also works without this plugin in ChatGPT and other AI apps that support MCP.
