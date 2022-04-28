<p align='right'>Ver. 1.220422<br>Author: Me aka FuckPutin INC<br>
Contributors:&nbsp;<a href="https://github.com/whitesheep">whitesheep</a>&nbsp;<a href="https://github.com/merlok">merlok</a>
  </p>

# Software threat model: a visual approach

***Table of contents***

1. [Prerequisite](#pre)

1. [About this approach](#abo)

1. [Why I don’t use automated tools](#noa)

1. [Secure development cycle](#sdl)

1. [Sample visual model using Miro](#sam)

1. [Workflow](#wor)

1. [Verification](#ver)

1. [References](#ref)

## Prerequisite <a name="pre"/>
To implement this approach, **you have to be confident on cyber threats in general**, **and to have specific knowledge about pen testing**, the more you have the better it is. If you don’t have such skills, it is advisable to engage one or more consultants to assist you in implementing SDL.

Also, you have to be confident to implement an Agile principle into your SDLC
## About this approach <a name="abo"/>
We don’t use any well-known methodology (PASTA, STRIDE, VAST and so on), rather we borrowed some VAST concepts. Our method is based on the COVID 19 APP threat model: <https://www.youtube.com/watch?v=tCG1m9CmAPo>

Since we truly believe that visual representation greatly helps to identify and analyze every process, we proceeded using Miro, that actually is the only one tool used.

Furthermore, we need to have a live model, integrated into our SDLC that is based on Agile, so we introduced the s*ecurity developer personas* and we managed the mitigation actions as user story (US). W**e have created US only for those mitigations that can be directly implemented into the code**, for those mitigation actions that are not under direct control of the DEV team, we use an approach of *educating and monitoring,* based on our own policies. You can use a different approach, the important concept here is to have a holistic approach: you have to take care of the whole system.

Since the SDL is an ongoing process, **we verify our model at the end of each software production iteration**. Implementing security requirements as user story, furthermore helps us to identify proper scenario in the validation of the acceptance criteria **using BDD example mapping.**

## Why I don’t use automated tools <a name="noa"/>
First, they cost quite a lot of money, but the main reason is that they produce a lot of false positive based on general mitigations, of course they don’t take into consideration the ecosystem where the application is deployed. They lack a holistic approach to manage SDL, for instance quite a lot of vulnerability requires some prerequisites to be exploited.Furthermore, these tools do not care about the worst threat: social engineering. They are mainly focused on assets and processes, without considering that very often security involves humans. 
## Secure development cycle <a name="sdl"/>
![](img/SDL.png)

*Please note that the picture above does not take into account the verification (testing) phase, we will introduce this phase after the **mitigation** step for sure*

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

![](img/board.png)

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
|6|FuckPutin inc.|Mitigated|We provide user training|
|7|Client|Mitigated|HTTPS has been enabled|
|8|FuckPutin inc.|Accepted|We have a DDOS response plane defined|
|9|Client|Transferred|We provided line guides for hardening the system|
|10.a|Client|Transferred|We provided line guides for DB table encryption|
|12|FuckPutin inc.|Accepted|Hiring policy<br>NDA<br>Logging and monitoring|
|13.b|<p>TS FuckPutin inc.</p><p></p>|Transferred|FuckPutin inc. continuous monitor for newly emerged threats|

## Verification <a name="ver"/>
![](img/bdd.png)

To verify the secure user stories, we use BDD with example mapping. The idea is to define the rules as acceptable criteria for the security requirements, starting from a real scenario (examples). This job has to be conducted as a brainstorming session, if possible, involving all the members of the DEV team. Moreover, this approach helps to reveal missed security requirements as new user stories, if you have any doubts, you can use the red note to trace them - as shown in the image above - and further discuss the issue.

## References <a name="ref"/>
- <https://owasp.org/www-pdf-archive//Threat-Modelling_oct2017.pdf>
- <https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html>
- <https://www.youtube.com/watch?v=tCG1m9CmAPo>
- <https://cucumber.io/blog/bdd/example-mapping-introduction> 
---
###### SDL: A visual approach
