# Holdpoint for Jira

Publisher: Intronode Korea Inc.

Support: [Support](./support.md)

Privacy policy: [Privacy policy](./privacy.md)

Holdpoint compares a supplier invoice on a Jira work item with a purchase order and a goods receipt. The comparison is code. A person, or the Holdpoint Exceptions Agent where Rovo is available, can draft one resolution. Nothing is posted to a supplier, and the payment status does not change, until a person approves it in the Holdpoint panel.

Holdpoint does not resend a supplier message on its own.

## Install

A site admin installs **Holdpoint for Jira** from the Atlassian Marketplace onto **Jira**. Jira Service Management on the same site is optional. Holdpoint uses it to post an approved message as a public reply on a customer request, and during sample setup to find a request type and create the fifth sample request.

The install screen lists the permissions. Why each one is needed:

| Permission | Why |
|---|---|
| Storage for this installation | Stores exceptions, the audit trail, policy, the policy change log, purchase orders, goods receipts, the duplicate index, and reminder markers |
| Read Jira work | Reads the invoice work item, its description, and its attachments, and reads comments when checking whether a message was already posted |
| Write Jira work | Writes comments, the payment and match properties, labels, and, when an administrator asks, the sample work items. On a service desk that includes a customer request |
| Read Jira users | Shows display names on a screen. Those names are not written into storage or into comments |
| Read service-desk requests | Checks whether a work item is a Jira Service Management customer request |
| Write service-desk requests | Posts an approved supplier message as a public reply on that request |

Holdpoint does not ask for permission to call a host outside Atlassian.

## Open the app

In the project, open **Holdpoint** from the project sidebar. The page has five tabs: **Queue**, **Policy**, **Reference data**, **Audit**, and **Getting started**.

On a work item, the **Holdpoint** panel sits below the description. If it is missing, look for the Holdpoint icon in the icon row under the work item title. After one click the panel appears below the description and the page scrolls to it. Then, optionally, the panel's ••• menu, **Show for all work items**, shows it for every work item in the project. Only Jira administrators can see **Show for all work items** and **Hide from all work items**. The panel can take several seconds to appear.

Only users who can edit the work item see the Holdpoint button. Approval itself requires that the user can view the work item and is an approver under the policy.

## Set up a project

1. Open **Queue** or **Getting started**.
2. Optional, and only for a demonstration: select **Set up the demo**. It loads fictional sample purchase orders, goods receipts, and four sample invoices, then matches them. On a service desk with a request type Holdpoint can use, it also creates a fifth work item as a customer request, if Jira allows the create. Running it again does not create a second work item for a slot that already has one. A project administrator or a Jira administrator can do this. Someone with neither permission sees a read-only message, and the button stays disabled. If Jira refuses that person on a new project, Holdpoint may create one of the four invoice work items as the app. The customer request is created only as the signed-in user.
3. For real invoices, import purchase orders and goods receipts on **Reference data**, then put the invoice on a work item as a Holdpoint CSV file attachment or as a CSV code block in the description.
4. On **Policy**, leave the defaults unless you mean to change them: price tolerance 2 percent, high-severity amount 1,000.00, reminder after 24 hours. Add approvers with **Add approver** and select **Save**. If the approver list is empty, a project administrator of that project or a Jira administrator can approve. A named list does not fall through to either administrator permission.
5. **Require a different approver** means the person who requested the proposal cannot approve it, and neither can the person who last saved the supplier message. Turn it off only when one person must both request and approve: on **Policy**, switch **Require a different approver** off and select **Save**.

Creating a work item runs the match when the CSV is already there. Adding the CSV as an attachment afterwards runs it again, because the app is subscribed to a new attachment as well as a new work item. Editing the description later does not. Use **Run match** in the panel.

## The approval rule

The agent and any MCP client can list exceptions, open one exception, run the match, propose a resolution, read approval status, and read policy. None of those tools approves, rejects, pays, sends, or closes.

A proposal waits in **Pending approval**. The person approves or rejects it in the panel. While **Require a different approver** is on (the default), the requester does not see **Approve**. Asking the agent to approve is refused.

**Approve** runs four steps:

| Step | What it does |
|---|---|
| Send supplier message | For a resolution that sends a message. On a Jira Service Management customer request, Holdpoint posts the approved message as a public reply. If it cannot post it publicly, the text goes to an internal comment for manual sending; if the result is unclear, an approver is asked to confirm. On any other work item, it writes an internal comment, "Approved supplier message (send manually)", and shows the text in the panel. **Accept variance** sends no message |
| Set payment status | Writes the payment-status property |
| Add label | Adds a label such as `holdpoint-credit_requested` |
| Post audit note | Writes an internal comment. The approver, the requester, and a person who confirmed a comment was posted are Jira mentions with the account id only. The approver's comment is included when one was entered. Jira sends its normal mention notification to the people mentioned |

If Jira does not answer clearly, the state is **Needs your confirmation**. Holdpoint does not resend on its own. An approver chooses **It was posted** or, from 15 minutes after the attempt, **Not posted: send again**. If the comment search is incomplete, **Not posted: send again** is not offered. The public reply's last line is a reference Holdpoint can find again. The audit note ends with the same kind of reference. Holdpoint looks for that reference, not for a person's name, before it would post again.

On a customer request, the approved message is normally posted as a public reply (otherwise it goes to an internal comment for manual sending), and the audit note stays internal.

## External MCP clients

This is optional, and it is a Preview Forge module. External access stays off until an organisation or site admin turns it on for Holdpoint:

admin.atlassian.com → the site → Connected apps → Holdpoint → Tools → **Expose tools to MCP-compatible apps**.

The server URL depends on where Holdpoint was installed from.

From the Atlassian Marketplace:

```
https://mcp.atlassian.com/forge/79f7c162-fb63-493a-8d2d-d04e30680f5e
```

From the installation link that Intronode provides:

```
https://mcp.atlassian.com/forge/618666fb-6daa-4f78-9919-a81aac8ffd39
```

The last part of each URL is the id of that app. The client signs in and acts with that person's Jira permissions. The client still cannot approve. Tool names shown to a client may carry an environment suffix, such as `_development` on a development install.

## Limits

- Invoice CSV: at most 500 lines including the header, and at most 1 MiB.
- The queue reads at most 500 exception records.
- A supplier message on a proposal is 1 to 1,200 characters. The rationale is 1 to 600 characters.
- Quantities are checked per invoice. They are not added up across several invoices for the same purchase order. A later invoice can show as matched when an earlier invoice already used the whole order.
- Duplicate detection uses the supplier and the invoice number. Two different invoice numbers are not treated as duplicates, even when they bill the same purchase order.
- Reference import does not re-run matches. Use **Run match** on each affected work item.
- The channel shows **Rovo agent** when the call comes from Rovo, and **Agent (MCP or other client)** otherwise. A missing Rovo marker is not proof that the call came from an MCP client.

## Known limitations

- The four sample invoices are ordinary Jira work items, so an approved message for them is kept for manual sending. On a service desk the fifth sample, when it is created, is a customer request, and Holdpoint posts an approved message for it as a public reply; if Jira refuses that reply, the text goes to an internal comment for manual sending. That request uses the request type "Emailed request" when the desk has it. Its description is one paragraph, and the match reads it. The sample difference is a unit price of 6.60 against 6.00 on the order, USD 90.00 at risk.
- An unclear comment outcome needs a person. Choosing **It was posted** when the message was not posted leaves it unsent. Choosing **Not posted: send again** when the search could not see an existing reply can post a second one. The panel shows the last search result next to the buttons. Holdpoint does not make that choice itself.
- With **Require a different approver** on, an approver who edits the supplier message cannot then approve it.
- The approve check cannot tell a person from browser automation signed in as that person.
- Policy changes are not stored as exception audit events. The policy change log keeps the last 50 saves, each with the time, the saver's account id, the approver account ids, and the different-approver switch. The Audit tab shows that save.
- The Rovo skill and the MCP module are Preview modules of Forge.
