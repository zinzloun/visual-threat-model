<p align='right'>Ver. 2.0</p>

# Software threat model: a visual approach

***Table of contents***

1. [Prerequisite](#pre)

1. [About this approach](#abo)

1. [Why we don’t use automated tools](#noa)

1. [Secure development cycle](#sdl)

1. [Sample visual model using Miro](#sam)

1. [Workflow](#wor)

1. [Verification](#ver)

1. [Refectoring](#rec)

1. [AWS Sample model](#aws)

1. [References](#ref)

## Prerequisite <a name="pre"/>
To implement this approach, **you have to be confident on cyber threats in general**, **and to have specific knowledge about pen testing**, the more you have the better it is. If you don’t have such skills, it is advisable to engage one or more consultants to assist you in implementing SDL.

Also, you have to be confident to implement an Agile principle into your SDLC
## About this approach <a name="abo"/>
We don’t use any well-known methodology (PASTA, STRIDE, VAST and so on), rather we borrowed some VAST concepts. Our method is based on the COVID 19 APP threat model: <https://www.youtube.com/watch?v=tCG1m9CmAPo>.
<br/>We also use the NIST data-centric threat model: https://csrc.nist.gov/CSRC/media/Publications/sp/800-154/draft/documents/sp800_154_draft.pdf and we use the ATT&CK matrix to help us to discovery malicious techniques. The matrix is very useful to support in indentifing malicious scenario with related use-cases. A very useful tool to help in this process is the Navigator: https://mitre-attack.github.io/attack-navigator

Since we truly believe that visual representation greatly helps to identify and analyze every process, we proceeded using <b>Miro</b>, that actually is the only one tool used.

Furthermore, we need to have a live model, integrated into our SDLC that is based on Agile, so we introduced the s*ecurity developer personas* and we managed the mitigation actions as secure user story (SUS). **We create SUS only for those mitigations that can be directly implemented into the code**, for those mitigation actions that are not under direct control of the DEV team (e.g. a webserver configuration), we provide guidelines about how to implement the mitigations to the service or asset owner. 
The following flowchart better explain the process:
![](https://raw.githubusercontent.com/zinzloun/visual-threat-model/main/img/TM-wf.jpg)

The truth behind the holistic approach is that there are part of the system - especially we are talking about the enviroment where the application will be deployed - that are not under our control, maybe they are in the early stage, the testing enviroment, but almost never in production. In this situation is essential to pursue a collaborative approach, team based, brainstorm your model and if you can involve the product owner to explore the business context.

Stay focused on technical threats and vulnerabilities without loosing the big picture entirely, conduct a pragmatic and risk-based analysis to prioritize threats. The diversity of perspectives is foundamental here.

Since the SDL is an ongoing process, **we verify our model at the end of each software production iteration**. Implementing security requirements as user story, furthermore helps us to identify proper scenario in the validation of the acceptance criteria **using BDD example mapping.**

## Why we don’t use automated tools <a name="noa"/>
First, they cost quite a lot of money, but the main reason is that they produce a lot of false positive based on general mitigations, of course they don’t take into consideration the enviroment where the application is deployed or the business context, the socio-political situation. They miss to contextualize the threat model with the business model, neither they can evalute the whole application context (e.g. system integration). Furthermore, these tools do not care about the worst threat: social engineering. They are mainly focused on assets and processes, without considering that very often security involves humans. 

## Secure development cycle <a name="sdl"/>
![](https://www.h-x.technology/wp-content/uploads/2021/02/Infogr-SDLC.png.webp)

<sub><sup>www.h-x.technology/services/secure-development-lifecycle</sup></sub>


## Sample visual model using Miro <a name="sam"/>
- Identify threat actors
- Identify assets
- Identify data flows
- **Identify main threats**
- **Prioritize** the threats weighting the **risk = (impact \* likelihood)**
- Risk can be
  - Accepted
  - Avoided
  - Mitigated
  - Transferred
- Think about how to mitigate attacks vector in general, not only for a single threat (e.g., input sanitization)

![](https://raw.githubusercontent.com/zinzloun/visual-threat-model/main/img/board.png)

## Workflow <a name="wor"/>
1. Design the architecture. Identify
   1. Zones
   1. Assets
   1. Data flow
1. Identify threat actors
1. Identify attack scenario
1. Identify threats using brainstorming sessions, assign an ID to them. Use yellow notes
1. **Validate** the risks, weight = **Likelihood** (1-3) \* Impact (1-3), where 

|Likelihood||Impact||
| :- | :- | :- | :- |
|1|Improbable|1|Low|
|2|Probable|2|Medium|
|3|Almost sure|3|High|

**Then we have the following Risk score table**

|1 to 3|Minor|
| :- | :- |
|4 to 6|Serious|
|6 to 9|Very serious|

1. Analyze the threats risk:
   1. Mitigate them (green note)
   1. Threats that cannot be mitigated directly into the development phase or that have been transferred (blue note)
   1. Accept them (red note)
1. Add tags based on the risk score to the threats (**M**inor, **S**erious, **V**ery **S**erious)
1. If possible, first mitigate common threats (identify them using the tag **Com**)
1. **Prioritized** risk resolution based on the score for the threats that can be mitigated into the code
1. Create the relative user story for the *security developer personas*:

|Threat.ID|As *security* developer|
| :- | :- |
|2|Since I fear that the CIA of the data can be compromised through input injections, I will sanitize all the user input|
|3|Since I fear that the CIA of the data can be compromised through insecure deserialization, I will not use serialized object|
|c4|Since I want to prevent access to the application features by unauthorized users, I will allow only authenticated requests   |
|5|Since I want to protect the API access, I will implement Json Web Token properly |
|9|Since I want users to access only their own data set, I will implement RBAC|
|10.b |Since I want to prevent malicious users from reading data directly on the backend, I will implement obfuscation when data is processed|
|11.a|Since I fear that an attacker could try to guess my credentials, I will implement a robust password policy|
|11.b|And I will also implement a proper lockout policy|
|11.c|Since I want to enforce the authentication mechanism, I will implement 2FA|
|13.a|Since I want to know which third-party libraries I use in my application, I will create an inventory of them|
|13.c|Since a new vulnerability is affecting a third-party library, I will patch it according to our defined policy|

6. For the threat that you can directly mitigate into the code keep track of them as follows:

|Threat.ID|Owner|State|Mitigation|
| :- | :- | :- | :- |
|1|Client|Transferred|The client will apply his own patch management policy|
|6|Us |Mitigated|We provide user training|
|7|Client|Mitigated|HTTPS has been enabled|
|8|Us|Accepted|We have a DDOS response plane defined|
|9|Client|Transferred|We provided line guides for hardening the system|
|10.a|Client|Transferred|We provided line guides for DB table encryption|
|12|Us|Accepted|Hiring policy<br>NDA<br>Logging and monitoring|
|13.b|Us|Transferred|TS continuous monitor for newly emerged threats|

## Verification <a name="ver"/>
![](https://raw.githubusercontent.com/zinzloun/visual-threat-model/main/img/bdd.png)

To verify the secure user stories, we use BDD with example mapping. The idea is to define the rules as acceptable criteria for the security requirements, starting from a real scenario (examples). This job has to be conducted as a brainstorming session, if possible, involving all the members of the DEV team. Moreover, this approach helps to reveal missed security requirements as new user stories, if you have any doubts, you can use the red note to trace them - as shown in the image above - and further discuss the issue. Finally include security requirements implementation into definition of done for each software iteration release (usually Sprint based).

## Refactoring <a name="rec"/>

Of course it would be ideal to review your model periodically, at least each time that any change occurs into the software, but it is not enough since new threats emerge everyday. An approach to mitigate this problem is to not consider threat modeling as a separate projectual activity but really integrated into development, so when a new features is introduced to the software try to think before to the possible risks that involves, I don't use the word threat here on purpose, since the threats should be brainstormed with the whole team starting analyzing the risks, a TDD approach should be used here.

## AWS sample model <a name="aws"/>
Following you can find a threat model for a simple AWS architecture
![](https://raw.githubusercontent.com/zinzloun/visual-threat-model/main/img/aws_sample_tm.jpg)

Here you can download the related [security check list](./doc/AWS_SecList_1.0.odt)

## References <a name="ref"/>
- <https://owasp.org/www-pdf-archive//Threat-Modelling_oct2017.pdf>
- <https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html>
- <https://www.youtube.com/watch?v=tCG1m9CmAPo>
- <https://martinfowler.com/articles/agile-threat-modelling.html#RabbitHoleWhatAboutNationStatesAnd0-day>
- <https://cucumber.io/blog/bdd/example-mapping-introduction> 
- <https://securityintelligence.com/posts/a-guide-to-easy-and-effective-threat-modeling>
---
###### SDL: A visual approach
