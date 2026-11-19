# Holdpoint for Jira: privacy policy

Effective date: 2026-11-19

Publisher: Intronode Korea Inc., 7-28 Sindorimro 11ra-gil, Guro-gu, Seoul 08204, Republic of Korea

Intronode, not Atlassian, is responsible for how Holdpoint processes this data.

Privacy and security contact: privacy@intronode.com

Other pages: [Documentation](./index.md) · [Support](./support.md)

## In short

- Holdpoint runs entirely on Atlassian's Forge platform. It makes no calls to any service outside Atlassian, and it has no analytics or tracking.
- It stores the invoice data you give it, the decisions people make about it, and the Atlassian account IDs of the people involved. It does not copy names, email addresses or profile pictures from user profiles. Text that people type into a proposal, a message, the reasoning or a rejection reason is stored and can contain names.
- Intronode does not receive a copy of your data.
- Uninstalling the app removes its stored data on Atlassian's schedule. What it wrote into your Jira work items stays in Jira until you delete it there.

## What Holdpoint does

Holdpoint compares a supplier invoice on a Jira work item with your purchase-order and goods-receipt records. When they disagree, a person or an AI agent can propose a resolution. Nothing is sent to a supplier and no payment status changes until a person approves the proposal in Jira.

To do this, Holdpoint checks each newly created work item on the site, and each work item that gets a new attachment, for an invoice in Holdpoint's CSV format in an attachment or in the description. Of the attachments, it reads only the invoice CSV it selects (the newest `.csv` file); a file over 1 MiB is rejected. Sample work items it created are skipped. If no invoice data is found, it stores no invoice record for that work item; a malformed CSV can leave an "invalid input" record. Its log can record the work item key, the result and timings.

## What Holdpoint stores

Holdpoint keeps its records in Forge hosted storage, which Atlassian operates for each site that installs the app.

| Record | What it contains | Personal data in it |
|---|---|---|
| Invoice exception | The invoice (supplier ID, invoice and purchase-order numbers, dates, lines, amounts), what was found, the proposed resolution with its message to the supplier and its reasoning, the approval or rejection with any comment, and a record of each step carried out | Account IDs of the person who proposed, anyone who edited the proposal, the person who decided, and anyone who confirmed that a message was sent. Any text that people typed |
| Audit trail | Time, who acted (an account ID, or the app itself), by which route (a Jira screen, the Rovo agent, another agent or MCP client, or Holdpoint's automatic match and reminder), and what happened, including the reason entered when a proposal is rejected | Account IDs. The rejection reason, which is text people typed |
| Policy and its change history | Price tolerance, thresholds, the list of approvers, reminder timing, and the last 50 changes | Account IDs of approvers and of the people who changed the policy |
| Reference data | Purchase orders and goods receipts that you load, or the fictional sample set | None by design. Supplier names are whatever you load |
| Housekeeping | An index of invoice numbers used to spot duplicates, a marker that a reminder was sent (kept two days), the list of sample work items, and which sample work items the demo checklist was dismissed for | None |

An Atlassian account ID is an identifier, not a name. When a screen shows a person's name, Holdpoint asks Jira for it at that moment and does not save it.

Text that people type into a proposal, the message to the supplier, the reasoning, a comment or a rejection reason is stored as entered, apart from leading and trailing spaces. It can contain anything, including names. Rejection reasons are also stored in the audit trail.

## What Holdpoint writes into Jira

Holdpoint adds the following to the work item. On a Jira Service Management request, the approved public reply is the only content Holdpoint posts to the customer. Its other comments there are internal notes for your agents, and the two properties and the label are not posted as comments. On any other work item, everyone who can see the work item can see all of it.

- **Comments.** On a Jira Service Management customer request, Holdpoint posts an approved supplier message as a public reply, which the customer on that request can see. If Jira refuses the public reply, the text goes to an internal comment for a person to send; if the result is unclear, an approver is asked to confirm. On any other work item the approved text is a comment for a person to send. Holdpoint also adds a note recording the decision and a reminder when a proposal has waited too long. When a proposal is rejected, the rejection is saved first; Holdpoint then tries to add an internal note. Where these notes refer to a person, they use a Jira mention, so Jira shows the person's current name and sends its usual mention notifications.
- **Two properties** on the work item that record the match result and the payment status, and a **label** such as `holdpoint-credit_requested`.
- **Sample work items**, only if an administrator chooses to load the demonstration data.

## Where the data is kept

Forge hosted storage is kept in the same location as your Jira site and moves with it if your administrator moves the site. According to Atlassian's [data residency page](https://developer.atlassian.com/platform/forge/data-residency/), that location is one of: Global (regions that Atlassian determines), Australia, Canada, the EU, Germany, India, Japan, Singapore, South Korea, Switzerland, the United Kingdom or the United States. Atlassian states that the code of a Forge app may occasionally run in a different location from the one where the data is stored.

## Who receives the data

- **Intronode** does not receive a copy of the stored records, and Atlassian's developer console does not show Intronode the records of a production installation. It does show Intronode which sites have Holdpoint installed, and the app's logs, described below.
- **Atlassian** operates Forge and Jira, where all of this data lives.
- **Nobody else, by default.** Holdpoint has no email, webhook, analytics or telemetry, and Intronode uses no sub-processors for this app. Holdpoint does not sell data.
- **Rovo.** If you use the Holdpoint agent in Rovo, the conversation takes place in your Atlassian product. Intronode does not receive it.
- **External AI clients, only if your administrator turns this on.** Access for external clients over the Model Context Protocol (MCP) is off until an organisation or site administrator enables it for Holdpoint. After that, when one of your users connects such a client, the information it requests (invoice figures, findings, proposal text, work item keys) goes to that client and to whatever AI service it uses. Leave the setting off if you do not want this.

## Logs

Holdpoint's logs record what happened: event names, work item keys, states, timings and result codes. When a request to Jira fails, the log line can include up to 300 characters of the error text that Jira returns, which Holdpoint does not filter; query strings are removed from the logged request path. Other failures keep the error's text. Intronode can see these logs in Atlassian's developer console for the past 30 days, together with the name of the site they came from. This access is on when the app is installed; an administrator can turn it off in Atlassian Administration (Connected apps, Holdpoint, Logs access).

## How long data is kept, and how to delete it

- Holdpoint's stored records stay for as long as the app is installed. The audit trail can only be added to; this version has no screen for deleting a single record.
- A project administrator or a Jira administrator can remove the sample data on the **Reference data** tab with **Remove sample data**. It deletes the sample work items that person is allowed to delete, and the sample records, including their exception records and the sample purchase orders and goods receipts. Their audit history stays.
- A few short-lived working records expire on their own while the app is installed (for example daily reminder markers); the policy history keeps the 50 most recent changes. Everything else Holdpoint has stored for your site is removed by uninstalling the app, which a site administrator does. Holdpoint has no clean-up step of its own at that point; Atlassian deletes the storage. Atlassian's documentation says Forge hosted storage is retained for 28 days after uninstallation. It is not restored automatically if the app is installed again; Atlassian re-links it only on a request made within 21 days of uninstalling and with your consent.
- Comments, properties, labels and work items in Jira are Jira data. They stay until someone deletes them in Jira, including after the app is uninstalled.

## Your requests

To ask about access to, correction of, or deletion of personal data, write to privacy@intronode.com. Because the data is held in your organisation's Atlassian site and not by Intronode, your site administrator can see Holdpoint's records in Jira and can correct or delete what Holdpoint wrote into Jira. In this version an administrator can remove the sample data on the **Reference data** tab; Holdpoint's other stored records are removed by uninstalling the app, and Intronode cannot read or change them. We will help your administrator with any request. Please do not put personal data in a public issue tracker. We aim to reply within five business days (Monday to Friday, excluding public holidays in the Republic of Korea).

## Changes to this policy

If this policy changes in a way that matters, the new version will be posted on this page with a new effective date.
