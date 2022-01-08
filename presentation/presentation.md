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

- \hyperlink{conclusion}{Conclusion}

# 

\section{Introduction}
\label{intro}

# Introduction

A citation here to test the markdown [@necula:pcc]

# Next page of the presentation

- We can include latex!

\begin{equation*}
\sigma = \frac{1}{2} \left( \frac{1}{\sqrt{2 \pi}} \exp \left( -\frac{1}{2} \left( x - \mu \right)^2 \right) \right)
\end{equation*}


# 

\section{Conclusion}
\label{conclusion}


# Conclusion

Images in markdown format!

![Haskell Brooks Curry](res/HaskellBCurry.jpg){ width=30% }


# References {.allowframebreaks}

\bibliographystyle{apalike}
