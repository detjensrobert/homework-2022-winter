---
csl: ieee.csl
bibliography: sources.bib
---

# CS 477 - Midterm

## Robert Detjens

---

## 1. Describe the steps you would take to properly collect digital evidence from a suspected crime so it will remain admissible in court. Describe the tools you would use, the processes you would follow, and what decisions you need to make and why. What are some of the most important things you can do? Under what circumstances would you choose to not do a live memory capture? What equipment would you need to bring with you to the crime scene?

In order for a piece of evidence to be admissible to court, it must have a full paper trail that shows it has been handled correctly throughout the entire process, and the analysis
thoroughly documented. This ensures that the process could be duplicated by another expert and have them arrive at the same result.

At the scene of the crime, if there is a physical component such as a laptop or thumb drive, several pieces of information must be recorded, including who retrieved it, where and when it was found, and the condition it was in. The collector must not open it on scene, and instead leave it untouched and let a digital forensics expert perform the investigation.
If the evidence is solely digital, e.g. access logs, it again must be documented who, what, and where it was collected from, and again not modified in any way. The artifact can then be passed to the forensic expert and the handoff recorded.

For the actual analysis, the expert should make an exact of the data. For a hard drive, this could be done via disk imaging software like FTK Imager on Windows, or direct-copy utilities such as `dd` on Linux. This ensures that the original media is left as untouched as possible to preserve it. This imaging should be documented, including what tools were used and who performed it.

To examine the contents of the artifact, the disk image can be mounted **read-only** to protect it from changes. For Windows forensics, Eric Zimmerman has a selection of tools to examine the registry and shellbags to see the history of the machine and configured programs, and Autopsy can be used to examine the contents of files on disk, including installed programs and browser information. All findings should be recorded, including what was used to find it, where it was found, and who performed the investigation.

The most important things to remember is to never work on the original artifact, always record what you are doing, and ensure that the steps are repeatable.

Live memory capture can be tricky as RAM is always changing based on what the computer is doing, If the action in question is currently in progress or just happened, a memory capture of the affected system may be useful, but if the action happened more than an hour or two prior, it is very likely that the computer's memory does not have anything relevant in it anymore and a capture would not be useful.

As forensics experts, our evidence is not often physical outside of storage media, so at the crime scene we would usually just need our laptop and a storage media to store the recovered artifacts on for later analysis.

## 2. Technology has the ability to help preserve privacy through the use of tools such encryption and anonymizing networks. Yet those very tools can also be used by criminals to hide their activities. In several recent cases, the federal government has approached Apple asking they break the encryption on phones used by the suspect in criminal cases; Apple has refused to do so, citing the greater good. Is the privacy and protection brought about by encryption worth the cost where human life might be at risk? Pick a side and support your argument with external citations.

Removing encryption from the internet will not actually improve the situation. Examining the the EARN IT bill introduced in 2020 and now again this year, this bill tries to force companies to remove encryption from their services in the name of preventing child pornography [@earnit]. This would not stop actors from using encryption anyways, as bad actors can perform the encryption outside of the messaging service, and send those pre-encrypted messages to each other. This would also hurt e.g. activists in a dictatorship where their government is actively oppressing free speech, since they would also not be able to use these services anymore without fear of their government coming from them. Removing encryption would only serve to move bad actors further off platform and use external services.

Adding government-only backdoors into encryption schemes, as the FBI wanted Apple to do in 2016 [@nyt], comes with its own severe problems. Any encryption scheme with a backdoor is no longer secure, as that backdoor will eventually fund its way into the hands of bad actors. This has happened before with the EternalBlue Windows exploit used in large-scale ransomware attacks being leaked from the FBI [@wannacry]. These backdoors, again, would not stop (informed) bad actors from using outside encryption without backdoors, which, again, does not improve the greater good as these communications are still encrypted, and yet the security of everything else using the backdoored scheme has been compromised.


### References
