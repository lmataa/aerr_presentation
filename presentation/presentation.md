---
title: "A Foundational Model for Types"
subtitle: ""
date: \today
institute: "Universidad Complutense de Madrid (UCM)"
author: 
- "Luis Mata"
- "Adrian Enríquez"
- "Carlos Ignacio Isasa"
fontsize: 10px
mathspec: true
bibliography: notes.bib
header-includes:
  - \titlegraphic{\vspace{5cm}\hspace*{6.3cm}\includegraphics[width=3cm]{./res/UCM_logo.png}}
  - \usetheme{metropolis}
  - \usepackage{bussproofs}
---

# Agenda


- \hyperlink{pcc}{Foundational Proof-Carrying Code}

- \hyperlink{foundational_types_1}{A Foundational Model for Types: fst. attempt}

- \hyperlink{foundational_types_2}{A Foundational Model for Types: snd. attempt}

- \hyperlink{conclusions}{Conclusions}

# 

\section{Foundational Proof-Carrying Code}
\label{pcc}

# Proof-Carrying Code

1. **Code producer**: generates executable $+$ proof that of adherance to a safety policy
   
2. **Code consumer**: validates the executable and the proof before running it.

Approached via type systems for machine code, not cryptography.

 [@necula:pcc]

# Foundational Proof-Carrying Code

\begin{center}
\textit{
  A framework for mechanical verification of safety 
  properties of machine language...
}
\end{center}

until here it's still Proof-Carrying Code

\begin{center}
\textit{
  ...with the smallest possible runtime and verifier.
}
\end{center}

and this last part is the Foundational one. 

 [@appel:fpcc]

# Foundational Proof-Carrying Code

## How?

- Choose a foundational logic (e.g. higher-order $+$ arithmetic).
- Model the machine architecture (e.g. a step relation $(r,m) \mapsto (r',m')$ between memory and register bank states).
- Model the security requirements (e.g. by making the step relation deliberately partial).
- Model a type system (e.g. values as memory data and types as sets of values).

# Foundational Proof-Carrying Code: Logic

They chose higher-order logic with some arithmetic axioms, 
and implement the specifications using the Twelf language.

# Foundational Proof-Carrying Code: Architecture

Example of an \textit{add} instruction 
encoding:

\begin{align*}
  \mathsf{add}(i, j, k) = \;\;\;\;&\\ 
    \lambda r.m.r'.m'.r'(i) &= r(j) + r(k) \\
      &\land (\forall x \neq i.\;r'(x) = r(x)) \\
      &\land m' = m \\
\end{align*}

# Foundational Proof-Carrying Code: Safety policy

Example of safety requirement specified in the semantics
of the previous instruction itself:

\begin{align*}
  \mathsf{add}(i, j, k) = \;\;\;\;&\\ 
    \lambda r.m.r'.m'.r'(i) &= r(j) + r(k) \\
      &\land (\forall x \neq i.\;r'(x) = r(x)) \\
      &\land m' = m \\
      &\land i \neq 42 \\
\end{align*}

# Foundational Proof-Carrying Code: Bad news

Nowadays this seems to have evolved to to prove safety and 
correctness with type systems for 
source code instead of machine code and they are 
trustworthy if compiled with a formally verified compiler
[@appel:fpcc:compilers] (e.g. CertiCoq 
[@website:certicoq], CompCert [@website:compcert], 
CertiKOS [@website:certikos]).

# Foundational Proof-Carrying Code: Type systems

However, this motivated a research about
encoding type systems in a foundational way, 
which is not only applicable to type systems for machine 
code as they show for example with a usual typed lambda 
calculus.

# 

\section{A Foundational Model for Types: fst. attempt}
\label{foundational_types_1}

# Fst. attempt

Values are defined in a straightforward way for the 
use case, and types are sets of values.

We are going to see a machine instruction example 
where values are memory data and adresses.

# Fst. attempt: Machine instructions PCC example

Pair projections:

\begin{prooftree}
\AxiomC{$m\vdash x:\tau_1\times\tau_2$}
\UnaryInfC{$m\vdash m(x):\tau_1\wedge m(x+1):\tau_2$}
\end{prooftree}

Adding safety policies into the mix:

\begin{prooftree}
\AxiomC{$m\vdash x:\tau_1\times\tau_2$}
\UnaryInfC{$\mathsf{readable}(x)
  \wedge\mathsf{readable}(x+1)
  \wedge m\vdash m(x):\tau_1\wedge m(x+1):\tau_2$}
\end{prooftree}

But this is PCC yet.

# Fst. attempt: Machine instructions FPCC example

Let us define $m\vdash x:\tau$ as an application of 
predicate $\tau$ on memory $m$ and integer $x$:

$$m\vdash x:\tau \equiv \tau (m)(x)$$ 

We write $\models_m x:\tau$ to note the change from a 
syntactic viewpoint to a semantic one.

# Fst. attempt: Machine instructions FPCC example

The previous rule defined as 

\begin{prooftree}
\AxiomC{$\models_m x:\tau_1\times\tau_2$}
\UnaryInfC{$\mathsf{readable}(x)
  \wedge\mathsf{readable}(x+1)
  \wedge \models_m m(x):\tau_1\wedge m(x+1):\tau_2$}
\end{prooftree}

stands for

\begin{align*}
  \mathsf{pair}(\tau_1,\tau_2)(m)(x) =
  &\;\mathsf{readable}(x)\\
  \wedge&\;\mathsf{readable}(x+1)\\
  \wedge&\;\tau_1(m)(x)\\
  \wedge&\;\tau_2(m)(x+1)
\end{align*}

# Fst. attempt: Results

Achievements:

- Usual type system features.
- Datatype declarations.
- Function types.

But they had trouble in encoding some highly desirable features:

- Recursive datatype definitions where the recursion is in contravariant position.
- Mutable fields.

# 

\section{A Foundational Model for Types: snd. attempt}
\label{foundational_types_2}

# Snd. attempt

A type is modeled as a set of pairs of index and value of the form 
$\langle k, v \rangle$.

It has to be closed under decreasing index:

$$
\langle k, v \rangle \in \tau \implies 
\langle j, v \rangle \in \tau \;\forall j \leq k 
$$

# Snd. attempt

Type sets defined for each use case, but here are some conventional 
examples:

\begin{align*}
  \bot &\equiv \{\} \\ 
  \top &\equiv \{ \langle k, v \rangle\;|\; k \geq 0 \}
\end{align*}

Type environment and states are modeled respectively as mappings 
from variables to types and variables to values. 
$\sigma :_k \Gamma$ denotes consistency:

$$
\sigma(x) :_k \Gamma(x) \; \forall x \in dom(\Gamma)
$$

# Snd. attempt

We write $e :_k \tau$ if $e \rightarrow^j v$ for some $j < k$ 
implies $\langle k - j, v\rangle \in \tau$, where $\rightarrow$ 
is the step relation given by the corresponding small step 
semantics.

With this, the entailment relation $\Gamma \models_k e$ 
is the semantic counterpart of a type judgement and means

$$
\sigma(e):_k \alpha \; \forall \sigma \;\text{s.t.}\; \sigma :_k \Gamma
$$

Also, $\Gamma \models e : \alpha$ means that the relation is 
satisfied for all $k \geq 0$.

# Snd. attempt: Lambda calculus example

$$              
  e ::= x 
      \;|\; 0                        
      \;|\; \langle e_1, e_2 \rangle 
      \;|\; \pi_1(e) 
      \;|\; \pi_2(e)     
      \;|\; \lambda x. e 
      \;|\; e_1\;e_2
$$

# Snd. attempt: Lambda calculus example

Values: $0$, closed abstraction terms and pairs of values.

Small step semantics: standard.

Safeness: to not reach a stuck term, trivially modeled from the 
definition of $\models e:\alpha$.

# Snd. attempt: Lambda calculus example

Type sets:

\begin{align*}
  \mathsf{int} &\equiv \{ \langle k, 0 \rangle \;|\; 
    \forall k.\;k \geq 0 \} \\
  \tau_1 \times \tau_2 &\equiv 
    \{ \langle k, \langle v_1, v_2 \rangle \rangle \;|\; 
          \forall j.\;j < k \land
          \langle j, v_1\rangle \in \tau_1  \land 
          \langle j, v_2 \rangle \in \tau_2
    \} \\
  \alpha \rightarrow \tau &\equiv 
    \{ \langle k, \lambda x. e \rangle \;|\; 
        \forall j.\;j < k \land
        \langle j, v \rangle \in \alpha \implies
          e[v/x] :_j \tau
    \} \\ 
  \mu F &\equiv 
    \{ \langle k, v \rangle \;|\; \langle k, v \rangle \in 
      F^{k + 1}(\bot)
    \}
\end{align*}

They are closed under decreasing index by definition.

# Snd. attempt: Lambda calculus example

## Type inference lemmas:

\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models x: \Gamma(x)$}
\end{prooftree}
\end{minipage}
\hfill
\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models 0: \mathsf{int}$}
\end{prooftree}
\end{minipage}

\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models f: \alpha \rightarrow \beta \;\;\;\; \Gamma \models e: \alpha$}
\UnaryInfC{$\Gamma \models f\;e : \beta$}
\end{prooftree}
\end{minipage}
\hfill
\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma[x := \alpha] \models e: \beta$}
\UnaryInfC{$\Gamma \models \lambda x. e: \alpha \rightarrow \beta$}
\end{prooftree}
\end{minipage}

\begin{prooftree}
\AxiomC{$\Gamma \models e_1: \alpha \;\;\;\; \Gamma e_2: \beta$}
\UnaryInfC{$\Gamma \models \langle e_1, e_2 \rangle: \alpha \times \beta$}
\end{prooftree}

\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models e: \alpha \times \beta$}
\UnaryInfC{$\Gamma \models \pi_1(e) : \alpha$}
\end{prooftree}
\hfill
\end{minipage}
\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models e: \alpha \times \beta$}
\UnaryInfC{$\Gamma \models \pi_2(e) : \beta$}
\end{prooftree}
\end{minipage}

\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models e: \mu F$}
\UnaryInfC{$\Gamma \models e: F(\mu F)$}
\end{prooftree}
\hfill
\end{minipage}
\begin{minipage}[t]{0.48\linewidth}
\begin{prooftree}
\AxiomC{$\Gamma \models e: F(\mu F)$}
\UnaryInfC{$\Gamma e : \mu F$}
\end{prooftree}
\end{minipage}

# 

\section{Conclusions}
\label{conclusions}

# Conclusions

Foundational Proof-Carrying Code, although out of interest 
nowadays, motivated a research about modeling type systems 
in a foundational way.

# Conclusions 

Their results may help in studying type systems from the point 
of view of foundational mathematics and also where the introduction 
of rules must be correct within a logic in a machine-checkable way.

# Conclusion

After several attempts, they modeled succesfully some basic and advanced
type system features:

- Usual type systems of functional languages.
- Usual type systems of imperative languages.
- Specific type systems for machine instructions.
- Function types (e.g. first-class, continuations and closures).
- Universal and existential quantification.
- Type equalities ($\tau_1 \sim \tau_2$).

# References {.allowframebreaks}

\bibliographystyle{apalike}
