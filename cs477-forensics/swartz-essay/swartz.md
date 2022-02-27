---
title: Aaron Swartz and Overprosecution
author: Robert Detjens
date: 2/20/2022

fontsize: 12pt
header-includes:
  - \usepackage{setspace}
  - \doublespacing

bibliography: sources.bib
csl: ieee.csl
---

# Background

Aaron Swartz was a promising young internet developer that was influential in helping shape the Internet in the 2000s,
and also for his prominent activisim around open access to information. Swartz was the co-founder of Reddit [@wired],
helped develop standards such as RSS and Markdown [@markdown], and campaigned in favor of keeping the internet free and
open, opposing the Stop Online Privacy Act (SOPA). Swartz was very outspoken on the subject of open access, having created
several projects promoting unrestricted access to government records. The PACER site for accessing government law
records was extremely obtuse and required the user to pay per-page to access the information [@documentary]. Swartz
contributed to a project started by Carl Malamud, the PACER Recycling Project: a re-uploading site that allowed people
to send in PACER documents from the few free access PACER locations in select libraries [@documentary]. Swartz improved
upon a script that automonously downloaded documents from PACER. This mass downloading caused the courts to get the FBI
involved, however, the FBI dropped the investigation after trying to contact Swartz [@wired].

Swartz also was involved in more traditional political activisim. He co-founded political activism group Demand
Progress, opposing the SOPA and online surveillance. Swartz was also directly involved in opposing SOPA, giving speeches
in protest and speaking out on talk news panels [@documentary]. Swartz also published the "The Guerilla Open Access
Manifesto", detailing the public's "duty" to share the information they have access to, to contribute to the open-access
movement as a "moral imperative" [@manifesto].

# The Incident

JSTOR is an academic journal publisher, and provides a platform for researchers to publish their papers in. Access to
these papers costs money to the general public, something that Swartz was known to be in opposition to from his work
with the PACER Recycling project and from the aggressive stance detailed in the Guerilla Manifesto. MIT provides open
access to the JSTOR academic journal for everyone on campus networks [@documentary].

On September 25, 2010, an IP address from MIT began mass-downloading PDFs from JSTOR. This IP belonged to a new laptop
Swartz had registered under the name `Gary Host`, and contained a python script called `keepgrabbing.py`. This IP was
blocked to prevent JSTOR's site from slowing down, but the next day another IP began mass-downloading as well. This
prompted JSTOR to block the entire MIT IP space from accessing JSTOR. Several weeks later in early October, downloading
began again, much faster than before. JSTOR began developing security measures forcing users to log in with MIT
credentials to access JSTOR accounts, hoping to block this user [@jstor]. The mass downloading continued throughout
December and into January, when JSTOR rolled out the login measures. At this point, the authorities got involved and
began investigating.

The authorities found Swartz's laptop in a network closet directly wired into networking infrastructure and downloading
articles from JSTOR onto an external hard drive. A surveillance camera was installed, and caught Swartz entering the
closet and doing something to the computer with a hard drive out of frame.

# The Charges

The federal government and FBI charged Swartz with wire fraud and accessing a protected computer under the Computer
Fraud and Abuse Act. An additional addendum to the original case added more charges under the CFAA and felony charges
for fraud.

# The Crime

Under the CFAA, accessing a protected computer without authorization or in excess, and accessing with the intent to
commit fraud are violations. The evidence strongly suggets that not only did Swartz knowingly bypass the restrictuons
imposed on him once JSTOR noticed his mass downloading, his prior experiences with mass downloading and academic
restrictions (PACER and the Guerilla Manifesto, respectively) implies that Swartz had intent to redistribute these
documents. Under the CFAA, this is indeed a violation, but a misdemeanor and not a felony.

I agree with Swartz that these documents should be provided free of charge to the public, however under the current set
of laws Swartz's actions were indeed in vilation of the law and a crime.

\pagebreak

# References

::: {#refs}
:::
