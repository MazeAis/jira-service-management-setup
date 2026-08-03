<p align="center">
<img src="https://i.imgur.com/U5o4lRr.png" alt="Jira Service Management"/>
</p>

<h1>Jira Service Management - Setup and Configuration</h1>
This tutorial outlines the setup and configuration of a functioning IT service desk in Jira Service Management, including request types, triage queues, SLAs, and a linked knowledge base.<br />


<h2>Environments and Technologies Used</h2>

- Jira Service Management (Atlassian Cloud)
- Confluence (linked knowledge base)
- Customer Portal (end-user self-service)

<h2>Platform</h2>

- Atlassian Cloud - Free plan (browser-based; no local OS dependency)

<h2>List of Prerequisites</h2>

- Atlassian account
- Jira Service Management site provisioned on the Free plan
- IT Service Management (ITSM) project template
- A second browser profile or private window (to act as a customer)
- Confluence space linked to the service project

<h2>Configuration Steps</h2>

<h3>Part 1 - Create your site and project</h3>

<p>
<img src="https://i.imgur.com/z3pXFzX.png" height="60%" width="60%" alt="Project creation screen with ITSM template selected"/>
</p>
<p>

</p>
<p>
Create your Atlassian site and select the Free plan for Jira Service Management. The site name becomes your permanent URL (<code>yoursite.atlassian.net</code>), so choose something professional.

The Free plan supports 3 agents, unlimited customers, 2GB storage, and 100 email notifications per day - more than enough to build and demonstrate a complete service desk.

The project was created using the <b>IT Service Management</b> template rather than the generic service desk template, because the ITSM template ships with incident, service request, change, and problem request types already scaffolded.
</p>
<br />

<p>
<img src="https://i.imgur.com/IUpI9y3.png" height="80%" width="80%" alt="Empty queue immediately after project creation"/>
</p>
<p>

</p>
<p>
This is the baseline. Everything that follows is configuration applied on top of the default template.
</p>
<br />

<h3>Part 2 - Agent view and customer portal</h3>

<p>
<img src="https://i.imgur.com/h6jorID.png" height="80%" width="80%" alt="Agent view and customer portal side by side"/>
</p>
<p>
</p>
<p>
JSM separates the two interfaces entirely:

- <b>Agent view</b> (<code>/jira/servicedesk/projects/...</code>) - queues, SLAs, internal comments
- <b>Customer portal</b> (<code>/servicedesk/customer/portal/...</code>) - what an end user sees when filing a ticket

This mirrors the admin and agent panel split in osTicket. The portal was kept open in a private browser window throughout the build, since every configuration change needs to be verified from the user's side as well as the agent's.
</p>
<br />

<h3>Part 3 - Build request types</h3>

<p>
<img src="https://i.imgur.com/JT5rk5B.png" height="80%" width="80%" alt="Configured request type list"/>
</p>
<p>
</p>
<p>
Four request types were created under <b>Project settings</b> → <b>Request types</b>, reflecting real help desk volume:

| Request type | Category |
| --- | --- |
| Reset my password | Logins and Accounts |
| Hardware not working | Computers |
| Request access to a system | Logins and Accounts |
| Software installation request | Applications |
</p>
<br />

<p>
<img src="https://i.imgur.com/rDE9DB3.png" height="80%" width="80%" alt="Field configuration for a single request type"/>
</p>
<p>
</p>
<p>
Portal fields were customized so that intake captures what triage would otherwise have to chase. "Hardware not working" prompts for asset tag, physical location, and whether the issue is fully blocking work - so urgency is captured at submission rather than guessed at later.
</p>
<br />

<p>
<img src="https://i.imgur.com/KP3NKm7.png" height="80%" width="80%" alt="Request type as rendered in the customer portal"/>
</p>
<p>
</p>
<p>
Verifying the portal rendering matters: fields that make sense to an administrator are not always clear to the person filling them in.
</p>
<br />

<h3>Part 4 - Configure queues</h3>

<p>
<img src="https://i.imgur.com/Atrtws4.png" height="80%" width="80%" alt="Configured queue list with ticket counts"/>
</p>
<p>
</p>
<p>
Queues in JSM are ordered saved filters that determine what an agent sees first on login. The following were created under <b>Project settings</b> → <b>Queues</b>:

- <b>Unassigned</b> - anything with no assignee
- <b>Breached SLA</b> - filtered on SLA breach
- <b>Waiting on customer</b> - status-based
- <b>My open tickets</b>

Ordering matters: the queue at the top is the one that gets worked first, so it should reflect what is most at risk rather than what is most recent.

</p>
<br />

<h3>Part 5 - Build SLAs</h3>

<p>
<img src="https://i.imgur.com/yII9RqZ.png" height="80%" width="80%" alt="SLA configuration showing goal, start, pause and stop conditions"/>
</p>
<p>
</p>
<p>
Two separate clocks were configured under <b>Project settings</b> → <b>SLAs</b>, because a service desk measures acknowledgement and resolution differently:

<b>Time to first response</b>
- Goal: 1 hour (Highest priority), 4 hours (all others)
- Start: issue created
- Pause: status = Waiting for customer
- Stop: first public comment by an agent

<b>Time to resolution</b>
- Goal: 4 hours (Highest priority), 3 business days (Low)
- Start: issue created
- Pause: status = Waiting for customer
- Stop: resolution set

A business-hours calendar was attached to the resolution SLA so the clock only runs during working hours. The pause condition matters just as much - when a ticket is waiting on the customer, the clock stops, so the team is not measured against someone else's delay.
</p>
<br />

<p>
<img src="https://i.imgur.com/FYHUUgb.png" height="80%" width="80%" alt="Ticket displaying a live SLA countdown"/>
</p>
<p>
</p>
<p>
Once configured, the SLA timer appears directly on the ticket, giving the agent immediate visibility of time remaining rather than requiring them to work it out.
</p>
<br />

<h3>Part 6 - Knowledge base</h3>

<p>
<img src="https://i.imgur.com/akYjfDF.png" height="80%" width="80%" alt="Knowledge base article surfacing in the customer portal"/>
</p>
<p>
</p>
<p>
A Confluence space was created and linked under <b>Project settings</b> → <b>Knowledge base</b>, then how-to articles were authored (password self-service reset, VPN connection steps, printer troubleshooting) and linked to their matching request types.

Once linked, articles surface in the customer portal as the user types - resolving common issues before a ticket is ever created. This is ticket deflection, and it is the difference between a service desk that absorbs volume and one that reduces it.
</p>
<br />

<h3>Part 7 - Working tickets end to end</h3>

Tickets were submitted through the customer portal from a separate browser session, then worked from the agent view. The distinction between internal and public comments is the one worth stressing: the internal thread is where the technical detail lives, and the public thread is where the plain-language explanation goes.
</p>
<br />

<p>
<img src="https://i.imgur.com/TvvYcYV.png" height="80%" width="80%" alt="Queue populated with tickets in mixed states"/>
</p>
<p>
</p>
<p>
Tickets were deliberately worked to different outcomes:

- Responded within SLA on several tickets; allowed one to breach to demonstrate what a breach looks like in the queue
- Moved a ticket to "Waiting for customer" to confirm the SLA clock paused
- Resolved and closed the remainder

A queue where every ticket is green demonstrates less than one that shows a breach and an explanation of what caused it.
</p>
<br />

<h3>Part 8 - Reporting</h3>

<p>
<img src="https://i.imgur.com/69Rluiz.png" height="80%" width="80%" alt="Created vs Resolved and SLA success rate reports"/>
</p>
<p>
</p>
<p>
Two built-in reports were reviewed under <b>Project</b> → <b>Reports</b> once ticket data existed:

- <b>Created vs Resolved</b> - whether the queue is keeping pace with intake
- <b>SLA success rate</b> - achieved versus breached

Reporting is what turns ticket closure into a view of where the queue is actually failing.
</p>
<br />

### ✅ Summary

This project demonstrates the end-to-end configuration of a Jira Service Management service desk on Atlassian Cloud, showcasing hands-on experience with:

Jira Service Management project setup using the ITSM template

Request type design with intake fields built to reduce follow-up

Triage queue configuration and prioritisation logic

Response and resolution SLAs with pause conditions and business-hours calendars

Confluence knowledge base integration for ticket deflection

Ticket lifecycle handling across both agent and customer portal views

The process covers service desk design, service level management, knowledge base authoring, and full ticket lifecycle handling - demonstrating practical ITSM and end-user support skills on a platform in active use across IT teams.
