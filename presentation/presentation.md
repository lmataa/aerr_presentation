---
title: "AERR presentation"
subtitle: "Something"
date: \today
institute: "Universidad Complutense de Madrid (UCM)"
author: 
- "Luis Mata"
- "Adrian Enríquez"
- "Carlos Ignacio Isasa"
- "Mieczyslaw Bak"
fontsize: 10px
mathspec: true
bibliography: notes.bib
header-includes:
  - \titlegraphic{\vspace{5cm}\hspace*{6.3cm}\includegraphics[width=3.5cm]{./res/UCM_logo.png}}
  - \usetheme{metropolis}
---

# Agenda

- \hyperlink{intro}{Introduction}

- \hyperlink{pcc}{Proof Carrying Code}

- \hyperlink{conclusion}{Conclusion}


# 

\section{Introduction}
\label{intro}

# Introduction

A type system is a set of rules that assigns a property called type to the various constructs of a computer program, such as variables, expressions, functions or modules [@wikipedia:type_systems].

# Introduction

A citation here to test the markdown [@necula:pcc]

# Next page of the presentation

- We can include latex!

\begin{equation*}
\sigma = \frac{1}{2} \left( \frac{1}{\sqrt{2 \pi}} \exp \left( -\frac{1}{2} \left( x - \mu \right)^2 \right) \right)
\end{equation*}



# 

\section{Proof Carrying Code}
\label{pcc}

# Proof Carrying Code (PCC)

Proof carrying code allows a system to be used to verify the correctness of a program by a formal verification method that accompanies the program.


# Proof Carrying Code (PCC)

...

# 

\section{Conclusion}
\label{conclusion}


# Conclusion

Images in markdown format!

![Haskell Brooks Curry](res/HaskellBCurry.jpg){ width=30% }


# References {.allowframebreaks}

\bibliographystyle{apalike}
