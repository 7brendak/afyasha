# afyasha
AfyaSHA Navigator 🏥

Bridging the information gap between Kenyan patients and the Social Health Authority (SHA) system  before they leave the house.


The Problem
In Kenya, the Social Health Authority (SHA) has replaced NHIF as the national health insurance scheme. But the rollout has created a dangerous information asymmetry: patients arrive at hospitals not knowing which facilities are active, what the OTP verification requirements are, or what workflow applies to their specific employment category.
This is not a theoretical problem. It happened to someone I know.
After falling ill, my friend and I visited two hospitals under SHA coverage. At the first, staff turned us away  not because the facility was inactive, but because their team hadn't been trained on the new civil servant SHA workflow. We had no way to know this before leaving the house. At the second facility, my friend couldn't receive care because he hadn't brought the specific mobile phone registered to his SHA account ,the one needed to receive the OTP for identity verification. We had to return home first.
He was in visible pain throughout.
Sitting in that second waiting room, I recognised that every hospital we visited had the clinical capacity to help. The failure was informational. And it was entirely preventable.
The same pattern was playing out at scale on social media. When the Linda Mama programme ended , the government scheme covering maternal care costs : I watched mothers posting frantically on X, crowdsourcing maternity costs and SHA coverage details through word-of-mouth. Pregnant women and emergency patients were navigating life-altering healthcare decisions through unverified threads because no structured, reliable, ground-truth source existed.
Administrative friction was being converted directly into clinical risk.

What AfyaSHA Navigator Does
AfyaSHA Navigator is an AI-powered tool that captures on-the-ground, real-time knowledge from people who actually work inside Kenya's health facilities :the pharmacists, nurses, and administrative staff who know how the system is functioning today — and makes that knowledge accessible to patients before they need emergency care.
A patient can query the system in plain language:

"Is Kenyatta National Hospital processing SHA claims for civil servants today?"
"What documents do I need for maternity care under SHA at a Level 4 facility?"
"Which hospitals near Westlands are active for SHA outpatient cover right now?"

The system returns verified, structured guidance — not social media noise.

Architecture & Key Design Decisions
Why RAG (Retrieval-Augmented Generation) over pure generative AI
My initial design used a purely generative LLM to answer patient queries. This failed in two ways:

Hallucination risk :a generative model confidently producing outdated SHA policy information is more dangerous than no information at all in a healthcare context
Latency and API costs :a purely generative approach at scale was too slow and expensive for users in low-bandwidth environments across Kenya

I pivoted to a Retrieval-Augmented Generation (RAG) architecture: the LLM answers questions by retrieving verified, contributor-sourced information from a structured knowledge base first, then generating a plain-language response grounded in that data. This keeps answers accurate, traceable, and lightweight.
This was a lesson in frugal innovation: technical sophistication must always be balanced with end-user accessibility. A brilliant model that times out on a 3G connection has failed its user.
Consensus-Verification Logic
The core challenge: how do you verify ground-truth information about hospital operations without access to a government database?
The system uses a consensus mechanism. When multiple contributors from different contexts report the same information about a facility — for example, three separate users confirming that Hospital X is processing civil servant claims on a given day  that convergence raises the confidence score of that information. A single unverified report stays flagged as low-confidence until corroborated.
This is borrowed from distributed systems thinking: you cannot trust any single node, but consensus across independent nodes produces reliable signal. Applied to healthcare information, it means the system gets smarter and more accurate with every contributor — without requiring central administrative control.
Low-Bandwidth First
Every design decision was made with a Kenyan patient on a mobile data connection as the primary user. This meant:

Lightweight frontend with minimal JS dependencies
Response caching for frequently queried facilities
Plain-language outputs that don't require health literacy to interpret


What I Learnt
Building AfyaSHA Navigator taught me something I now hold as a core design principle:
In low-resource health systems, the most critical data isn't stored in an official SQL database. It is latent in the minds of frontline staff.
The nurse who knows that the SHA system goes down every Friday afternoon. The pharmacist who knows which OTP workaround the hospital is using this week. The receptionist who knows which civil servant categories are currently being processed. That knowledge exists — it just isn't captured anywhere a patient can access it before they arrive in pain at a door that can't help them.
AfyaSHA Navigator is an attempt to systematically capture that informal intelligence and make it actionable.

Current Status

✅ Core RAG architecture implemented
✅ LLM integration with plain-language query handling
✅ Contributor input and consensus-scoring logic
✅ Tested with initial SHA facility queries
🔧 Optimising for low-bandwidth performance
🔧 Expanding knowledge base coverage across counties
📋 Next: integrating with SHA's official facility registry as a base layer


Why This Matters Beyond Kenya
SHA Navigator is a proof of concept for a broader class of problem: health system navigation in low-resource settings where official information is delayed, incomplete, or inaccessible to patients.
The same information asymmetry that stranded my friend outside a Nairobi hospital exists across health systems throughout sub-Saharan Africa. The administrative complexity of insurance transitions, the gap between official policy and ground-level implementation, the reliance on word-of-mouth for life-critical decisions — these are not uniquely Kenyan problems.
A pharmacist with clinical credibility, technical capability, and lived experience of these systems is well-positioned to build solutions that actually work in the settings they're designed for. That is what this project represents.

Built By
Brenda Koech — BPharm, USIU-Africa | Pharmacovigilance Associate
Combining clinical pharmacy expertise with self-taught data science and AI development in service of African health systems.
