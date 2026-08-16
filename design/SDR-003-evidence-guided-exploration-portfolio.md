# SDR-003 — Evidence-Guided Exploration Portfolio

**Status:** Proposed / research direction  
**Date:** 2026-08-16  
**Origin:** Andrew Ng M3-W3-17, epsilon-greedy policy, extended to scientific and engineering exploration  
**Scope:** scientific-research-harness; reusable by MMALS, Evidence-Guided Falsification Recommender, Test Authority and innovation governance  

## 1. Decision

Adopt an **Evidence-Guided Exploration Portfolio (EGEP)** as a methodological direction for allocating a bounded part of experimental capacity to uncertain, novel, contradictory, human-proposed, or apparently improbable hypotheses.

The portfolio shall not optimize only the value predicted by the current model. It shall preserve several distinct reasons to run an experiment:

1. exploit an already credible opportunity;
2. reduce important uncertainty;
3. falsify a dominant claim or resolve a contradiction;
4. explore a novel behavior, mechanism, or region of the design space;
5. test a human intuition, including one proposed by a non-expert;
6. preserve strategic optionality;
7. admit a small, explicitly bounded lane for **productive madness**: ideas that look irrational under the current representation but could reveal that the representation itself is incomplete.

The governing doctrine is:

> **Evidence-guided, judgment-accountable, and deliberately open to surprise.**

Or, more operationally:

> There is an institutional right to propose an improbable hypothesis, but no right to expose the real system, people, funds, or public interest to unbounded risk.

## 2. Why this is needed

Evidence-based governance can become self-sealing when it funds only what the current evidence, metrics, or model already recognizes. A value model cannot correctly value what it cannot represent. An idea may therefore appear mathematically “illogical” because:

- the objective function is incomplete;
- the action or design space excludes it;
- the uncertainty estimate is poor;
- the model has never observed the relevant regime;
- the available data encode historical choices and blind spots;
- the effect is delayed, nonlinear, systemic, or only valuable in combination;
- the organization confuses absence of evidence with evidence of absence.

This does not mean that every unconventional idea is good. It means that the current ranking is a **fallible belief state**, not an oracle.

The portfolio protects against two symmetric failures:

- **premature convergence:** always choosing the apparently best-known option;
- **innovation theatre:** celebrating novelty without disciplined learning, cost control, or accountability.

The target is not maximum randomness. It is **structured exposure to informative surprise**.

## 3. Origin in epsilon-greedy reinforcement learning

While a Q-network is still learning, the agent must act even though its estimate \(\widehat Q(s,a)\) is imperfect. A purely greedy policy selects:

\[
a_t = \arg\max_a \widehat Q(s_t,a).
\]

If random initialization makes an action look bad, the agent may never try it. It then collects no evidence capable of correcting that belief:

\[
\text{action never tried}
\Rightarrow
\text{no experience}
\Rightarrow
\text{estimate never corrected}.
\]

An epsilon-greedy policy breaks this loop:

\[
a_t=
\begin{cases}
\arg\max_a \widehat Q(s_t,a), & \text{with probability }1-\varepsilon,\\
\text{random action}, & \text{with probability }\varepsilon.
\end{cases}
\]

In Andrew Ng's example, \(\varepsilon=0.05\): 95% exploitation and 5% random exploration. Training often begins with a larger \(\varepsilon\), possibly 1, and reduces it toward a floor such as 0.01 as experience accumulates.

A useful nuance: if the exploratory action is drawn uniformly from \(m\) actions, it can also be the greedy action. Its total selection probability is then:

\[
P(a_{\mathrm{greedy}})=1-\varepsilon+\frac{\varepsilon}{m}.
\]

With four actions and \(\varepsilon=0.05\), this equals 0.9625.

Epsilon-greedy selects the **behavior used to collect experience**. It is distinct from the Bellman target:

\[
y_t=r_t+\gamma\max_{a'}Q(s_{t+1},a').
\]

The analogy for research and innovation is direct but not literal: a portfolio should not always fund only the experiment with the highest current expected value. However, uniform randomness is usually too wasteful for costly real-world experiments. More directed methods are needed.

## 4. Exploration is not one thing

| Exploration mode | Selection principle | Strength | Principal limitation |
|---|---|---|---|
| Epsilon-greedy | Random choice with probability \(\varepsilon\) | Simple; defeats permanent blind spots | Undirected and potentially wasteful |
| Softmax / Boltzmann | Probability increases with estimated value | Tries plausible alternatives proportionally | Poor Q-calibration can distort probabilities |
| Optimism / UCB | Estimate plus uncertainty bonus | Directs trials toward promising unknowns | Requires credible uncertainty estimates |
| Thompson / posterior sampling | Sample a plausible model, then act optimally for it | Randomness remains coherent with beliefs | Sensitive to posterior/model misspecification |
| Deep exploration | Commit to coherent multi-step hypotheses or policies | Can discover delayed and sparse rewards | More complex than stepwise dithering |
| Curiosity / intrinsic motivation | Reward prediction error, information gain, or learning progress | Seeks informative states | Can be trapped by irreducible noise (“noisy TV”) |
| Novelty search | Reward behavioral difference | Escapes deceptive objectives | Novelty alone does not imply utility |
| Quality-Diversity | Preserve diverse, locally good solutions | Builds a repertoire rather than one winner | Needs meaningful behavioral descriptors |
| Active learning / experimental design | Query the observation with high expected information | Efficient for learning a model or boundary | Information may not equal operational value |
| Falsification-directed exploration | Attack a consequential claim or mechanism | Reduces confirmation bias | Can neglect generative opportunity if used alone |
| Human-guided exploration | Admit demonstrations, preferences, interventions, and hypotheses | Injects tacit knowledge and reframes the space | Authority, bias, and group dynamics must be governed |

### 4.1 Softmax and structured distributions

Instead of selecting uniformly at random:

\[
P(a\mid s)=
\frac{\exp(\widehat Q(s,a)/\tau)}
{\sum_b\exp(\widehat Q(s,b)/\tau)}.
\]

A high temperature \(\tau\) spreads probability; a low temperature approaches greedy selection. For continuous controls or parameters, exploration may sample around the preferred action or from multiple distributions. For categorical actions, “slightly left or right” has no meaning unless a genuine geometry or similarity relation has been defined.

### 4.2 Uncertainty-directed exploration

A simple optimistic score is:

\[
\operatorname{score}(a)=\widehat Q(s,a)+\beta\,\sigma_Q(s,a),
\]

where \(\sigma_Q\) represents epistemic uncertainty and \(\beta\) controls the value assigned to learning. Thompson sampling instead draws a plausible model from a posterior distribution and chooses its preferred action. Randomized value functions and Bootstrapped DQN aim at coherent, temporally extended exploration rather than isolated random actions.

The important governance lesson is that **uncertainty is a reason to investigate, not automatically a reason to reject**. Yet uncertainty must not be manufactured or rewarded for its own sake.

### 4.3 Curiosity, information gain, and surprise

An intrinsic reward can augment the task reward:

\[
r_{\mathrm{total}}=r_{\mathrm{task}}+\eta r_{\mathrm{intrinsic}}.
\]

Possible intrinsic signals include prediction error, expected information gain, Bayesian surprise, reduction in posterior entropy, or learning progress. “Positive surprise” is valuable, but scientifically a negative surprise or a failed prediction may be equally important because it invalidates a mechanism or exposes a hidden regime.

Raw prediction error is unsafe as a universal curiosity signal. A stochastic but useless process can remain perpetually unpredictable. The portfolio must distinguish:

- epistemic uncertainty that additional evidence may reduce;
- aleatoric variability that may not be reducible;
- model inadequacy;
- measurement noise;
- genuine regime change.

### 4.4 Novelty Search and Quality-Diversity

Novelty Search asks how behavior differs from what has already been discovered. Quality-Diversity methods such as MAP-Elites retain a repertoire of high-performing but behaviorally distinct solutions. They are especially relevant when:

- the objective is deceptive;
- several operational niches matter;
- robustness comes from alternative mechanisms;
- the organization needs options, not a single global winner;
- a solution that is weak today may become valuable under another regime.

The behavioral descriptor is itself a governance choice. If it is badly chosen, the system will generate diversity in irrelevant dimensions.

## 5. Human intuition is an admissible exploration source

Any actor may contribute a candidate hypothesis: domain expert, operator, user, junior engineer, maintainer, supplier, researcher, customer, auditor, or outsider. Expertise affects the evidence and confidence attached to a proposal, but should not determine whether the proposal is allowed to exist.

Human input can enter as:

- a new hypothesis or mechanism;
- a previously excluded action;
- a proposed experiment or trajectory;
- a preference between outcomes;
- a demonstration;
- a warning or veto condition;
- a claim that the model's variables are wrong;
- a new behavioral descriptor for novelty;
- a request to preserve an option despite low present expected value.

Methods related to this include preference-based reinforcement learning, learning from demonstrations, human intervention during exploration, interactive imitation learning, and active learning that asks people to judge the most informative cases.

The inclusion rule is:

> **Every actor may propose; no actor receives an unlimited risk budget merely by proposing.**

To prevent an “open suggestion” mechanism from becoming symbolic, the ledger should record who proposed what, whether it was evaluated, why it was rejected or deferred, and what evidence could reopen it.

## 6. The productive-madness lane

### 6.1 Purpose

Reserve a small part of the portfolio for hypotheses that cannot initially win under the dominant scoring model but could produce disproportionate upside or reveal a missing representation.

“Madness” here does not mean recklessness, illegality, disregard for people, or exemption from engineering discipline. It means controlled admission of ideas with one or more of these properties:

- low prior probability but very high upside;
- high disagreement among credible viewpoints;
- no established measurement category;
- cross-domain analogy without direct precedent;
- a mechanism that contradicts a dominant assumption;
- a new combination of known components;
- a proposal originating outside the recognized expert hierarchy;
- an idea whose primary value is opening future options.

### 6.2 Barbell principle

The lane should combine **intellectual audacity** with **operational conservatism**:

- broad freedom in simulation, paper analysis, replay, prototype, and sandbox;
- narrow blast radius, strict stop conditions, and reversibility in the real world.

This creates asymmetric exposure:

\[
\text{bounded downside} \quad + \quad \text{preserved disproportionate upside}.
\]

### 6.3 Admission test

A wildcard does not need a strong expected-value case. It needs a minimal exploration case:

1. What current assumption could it overturn?
2. What observation would make it less implausible?
3. What is the smallest credible test?
4. What is the maximum loss, blast radius, and ethical exposure?
5. Can the test be stopped or reversed?
6. What will be learned even if it fails?
7. Who owns the judgment to proceed?

The idea must remain falsifiable enough to learn from. “It will work eventually, whatever the result” is not an admissible wildcard.

## 7. Portfolio architecture

An initial allocation, explicitly illustrative rather than normative, is:

| Lane | Purpose | Illustrative capacity |
|---|---|---:|
| Exploitation | Improve or scale already credible solutions | 60% |
| Uncertainty and falsification | Resolve consequential unknowns and attack fragile claims | 20% |
| Novelty and diversity | Explore different mechanisms and preserve repertoires | 10% |
| Human wildcards / productive madness | Protect atypical, low-prior, high-upside intuitions | 10% |

The allocation is a capacity guardrail, not a promise to spend. It must vary with criticality, reversibility, cost of delay, maturity, and the cost of real-world failure. A safety-critical production system may allocate most exploratory capacity to simulation and digital twins, whereas a low-risk prototype may allow more live exploration.

Budgets may be expressed in several currencies:

- money;
- engineering time;
- compute and energy;
- access to scarce data;
- user or operator attention;
- operational traffic;
- permitted blast radius;
- ethical and reputational exposure;
- calendar time and opportunity cost.

No single percentage captures all these budgets.

## 8. Candidate representation and selection

### 8.1 Minimum proposal card

Each candidate should contain:

- identifier and version;
- proposer and affected stakeholders;
- hypothesis or intuition in plain language;
- current model or assumption challenged;
- expected benefit and plausible extreme upside;
- uncertainty type;
- novelty relative to what has already been tested;
- disconfirming observation;
- cheapest informative experiment;
- required resources;
- hazards, constraints, and forbidden outcomes;
- stop, rollback, and escalation conditions;
- evidence produced if successful, null, contradictory, or failed;
- decision owner and review date.

The submission burden should remain low enough that unconventional ideas are not filtered out by administrative fluency. The qualification burden increases only as exposure increases.

### 8.2 Multi-criteria model

A conceptual score may be written as:

\[
S(x)=
\mathbb E[U(x)]
+\beta I(x)
+\lambda N(x)
+\rho H(x)
+\omega O(x)
-C(x)
-\kappa R(x),
\]

where:

- \(\mathbb E[U]\): expected utility under current beliefs;
- \(I\): expected information or uncertainty reduction;
- \(N\): novelty or contribution to diversity;
- \(H\): human hypothesis signal, including disagreement;
- \(O\): option value opened by the experiment;
- \(C\): cost;
- \(R\): downside risk.

This equation is a reasoning aid, not a universal truth. Collapsing all criteria into a single scalar can hide value conflicts and create false precision. Prefer:

- explicit constraints for non-negotiable safety and ethics;
- Pareto-front comparison for competing benefits;
- scenarios and sensitivity analysis for uncertain weights;
- a separate protected wildcard lane so that low-prior ideas do not have to defeat exploitation projects on exploitation's own metric;
- recorded human judgment for the final allocation.

### 8.3 Value of information

An experiment may be valuable even when it is unlikely to produce a directly deployable solution. Its value can come from changing a later decision. A simplified test is:

\[
\operatorname{EVSI}(x)
=
\mathbb E[\text{best decision value after observing }x]
-\text{best decision value now}
-\operatorname{cost}(x).
\]

The portfolio should also recognize the value of avoiding an expensive mistaken commitment, not only the value of discovering a winner.

## 9. Progressive exposure: from idea to bounded real POC

Use an escalation ladder. Passing one level grants only the next level, not production approval.

| Level | Environment | Typical evidence | Exposure rule |
|---|---|---|---|
| L0 | Thought experiment / literature | mechanism, prior evidence, counterexamples | no operational exposure |
| L1 | Historical replay / synthetic data | reproducible analysis, falsification attempts | no live decisions |
| L2 | Simulation / digital twin | sensitivity, stress cases, failure envelope | validate simulator assumptions |
| L3 | Sandbox / isolated prototype | feasibility, integration, basic hazards | isolated data and interfaces |
| L4 | Shadow mode | comparison with real flow without control | no effect on users or production decisions |
| L5 | Limited-budget POC | causal or operational evidence on a small slice | hard cap on time, spend, population, traffic, and loss |
| L6 | Controlled pilot | representative use with supervision | rollback, monitoring, named accountable owner |
| L7 | Scale decision | evidence pack and residual uncertainty | separate governance decision |

Before any real POC, define:

- maximum duration and spend;
- maximum population, traffic, geography, or system scope;
- safety envelope and prohibited actions;
- monitoring latency;
- stop-loss thresholds;
- manual intervention authority;
- rollback method and recovery time;
- data protection and consent requirements;
- success, null, contradiction, and harm outcomes;
- the decision that the POC is intended to inform.

“POC” must not become a label used to bypass production controls.

## 10. Evidence and learning obligations

Every funded exploration produces a versioned evidence record, including negative and inconclusive results. At minimum:

- hypothesis and prior belief;
- reason for allocation to a portfolio lane;
- protocol and preregistered decision criteria where feasible;
- environment, data, code, configuration, and seed identifiers;
- deviations from protocol;
- outcomes and uncertainty;
- surprises, anomalies, contradictions, and harm signals;
- posterior belief or revised confidence;
- decision: continue, replicate, redesign, falsify, pause, stop, or archive;
- reusable artifacts and lessons;
- date at which the conclusion should be reconsidered.

The governing loop is:

\[
\text{competing hypotheses}
\rightarrow \text{belief state}
\rightarrow \text{experiment proposal}
\rightarrow \text{bounded execution}
\rightarrow \text{evidence integration}
\rightarrow \text{human judgment}
\rightarrow \{\text{continue, replicate, falsify, stop}\}.
\]

Negative evidence is not a failed deliverable. It is an asset when it is credible, traceable, and capable of preventing repeated error.

## 11. Governance

### 11.1 Rights and responsibilities

- **Proposer:** may originate an idea without holding decision authority.
- **Experiment steward:** improves testability and identifies the smallest informative test.
- **Risk/ethics owners:** define hard constraints and prohibited exposure.
- **Decision owner:** accepts residual uncertainty and allocates the bounded budget.
- **Independent challenger:** attempts to expose optimistic assumptions and weak measurements.
- **Evidence custodian:** ensures reproducibility, versioning, and preservation of null/negative findings.
- **Affected actors:** contribute constraints, preferences, and lived operational knowledge.

No algorithm issues an authoritative GO/NO-GO decision. A recommender may rank, diversify, or explain candidates, but accountable people own the decision.

### 11.2 Judgment-accountable decision record

For each allocation, record:

- which decision is being made;
- which evidence and model informed it;
- which model limitations were known;
- why this lane and budget were selected;
- who disagreed and why;
- which residual risks were accepted and by whom;
- what would trigger review, interruption, or reversal.

### 11.3 Portfolio-level review

Review not only project outcomes but the health of exploration:

- Are wildcard funds repeatedly captured by senior actors?
- Do junior, operational, or external proposals receive a real hearing?
- Are experiments diverse in mechanism or merely in presentation?
- Does the organization terminate weak ideas early without punishing the proposer?
- Are successful surprises incorporated into the mainstream model?
- Are null and negative findings searchable and reused?
- Is exploration capacity quietly consumed by disguised exploitation work?

## 12. Metrics

No single KPI should control the portfolio. A balanced evidence set may include:

### Outcome and option metrics

- realized value and credible leading indicators;
- number and diversity of viable mechanisms retained;
- options opened, exercised, expired, or abandoned;
- robustness across regimes and scenarios.

### Learning metrics

- uncertainty or entropy reduced;
- consequential claims falsified or strengthened;
- decisions changed by evidence;
- learning per euro, per compute unit, or per unit of exposure;
- proportion of negative findings reused;
- time from surprise to model revision.

### Exploration-health metrics

- behavioral diversity, not merely count of ideas;
- share of proposals from different actor groups;
- protected-lane allocation and actual use;
- premature-convergence indicators;
- duplication avoided through the evidence ledger.

### Safety and discipline metrics

- stop-condition violations;
- unplanned blast radius;
- rollback success and recovery time;
- unresolved ethical or data-protection issues;
- experiments that failed to produce interpretable evidence.

Innovation success rate alone is dangerous. If it is optimized, teams will relabel safe incremental work as exploration and suppress high-uncertainty trials.

## 13. Failure modes and countermeasures

| Failure mode | Consequence | Countermeasure |
|---|---|---|
| Pure greedy allocation | Self-confirming portfolio; blind spots persist | Protected exploration lanes and periodic challenge |
| Pure random exploration | Waste and avoidable harm | Directed uncertainty, novelty, falsification, and staged exposure |
| Novelty theatre | Many unusual ideas, little learning | Falsifiable hypothesis and evidence obligation |
| “Noisy TV” curiosity | Resources consumed by irreducible unpredictability | Reward learning progress or reducible uncertainty, not raw surprise alone |
| Expert capture | Tacit hierarchy filters out weak signals | Open proposal rights, recorded disposition, rotating independent review |
| False democratization | Suggestions collected but never evaluated | Transparent ledger and response SLA |
| Scalar-score technocracy | Hidden value judgments appear objective | Hard constraints, Pareto views, sensitivity, and explicit human judgment |
| Goodhart's law | Teams game novelty, success, or information KPIs | Metric portfolio, qualitative review, periodic metric retirement |
| Unsafe “move fast” rhetoric | People or systems bear uncontrolled downside | Barbell principle, stop loss, reversibility, named owner |
| Endless POCs | Optionality without decisions | Decision-linked experiments and expiry dates |
| Punishing failed trials | Teams hide negative evidence and stop proposing | Separate disciplined failure from negligent execution |
| Survivorship and publication bias | Organization learns only from visible wins | Mandatory archive of null, negative, and abandoned trials |
| Model monoculture | All candidates share the same representation | Multiple models, human reframing, and mechanism diversity |
| Premature epsilon decay | Exploration ends before beliefs become credible | Review exploration rate against learning maturity, not calendar alone |

## 14. Relationship to existing research directions

### Scientific Research Harness

EGEP extends the harness from reproducibility and evidence traceability to **explicit governance of what gets investigated**. It should reuse preregistration, replayability, claim tracking, experiment versioning, and decision records.

### MMALS

MMALS can represent the belief state and regimes such as stable, transition, emergent, contradiction, or convergence. EGEP determines how experimental capacity is distributed across exploitation, targeted collection, replication, falsification, and exploration. This is a methodological direction, not yet a validated autonomous scientific-discovery agent.

### Evidence-Guided Falsification Recommender (EGFR)

EGFR may propose traceable, falsifiable candidate bundles under explicit cost, risk, and uncertainty. EGEP broadens the selection objective so that falsification, novelty, human intuition, and option value can coexist. It must not claim to identify the true or globally optimal next test and must not issue authoritative release decisions.

### Test Authority and engineering governance

The pattern can be used to protect a bounded capacity for mutation tests, abnormal scenarios, alternative integration strategies, operational intuitions, and low-probability/high-impact claims. In safety-critical contexts, exploration should occur primarily in model, replay, simulation, synthetic environments, and isolated benches before any live exposure.

## 15. Non-goals

EGEP is not:

- a claim that randomness is innovation;
- a requirement to run every proposed experiment;
- a mathematical proof that an idea is valuable;
- a way to bypass safety, law, ethics, architecture, or operational ownership;
- an autonomous funding or GO/NO-GO mechanism;
- a guarantee that portfolio percentages are universally correct;
- a validated scientific-discovery agent;
- a substitute for causal design, measurement quality, or expert review;
- a justification for exposing others to risks they did not accept.

## 16. Initial implementation experiment

Pilot EGEP on a bounded set of research or engineering candidates for three review cycles.

1. Create a common proposal card and evidence ledger.
2. Classify existing candidates retrospectively into the four lanes.
3. Allocate a small protected wildcard budget.
4. Require L0–L3 evaluation before considering any live POC.
5. Compare a greedy ranking against an EGEP portfolio using expected value, uncertainty, novelty, mechanism diversity, cost, and risk.
6. Record which candidates the greedy method would have excluded.
7. Run selected low-exposure experiments.
8. Evaluate whether the portfolio changed decisions, reduced important uncertainty, exposed model limitations, or preserved a useful option.
9. Conduct an ablation: remove the wildcard lane and assess what would have been lost.
10. Revise allocations and criteria; do not institutionalize the initial percentages without evidence.

Candidate research questions:

- Does a protected wildcard lane produce genuinely different mechanisms or merely lower-quality variants?
- Which novelty descriptors correlate with later usefulness?
- Can learning-per-unit-risk be estimated credibly?
- How should expert and non-expert intuition be calibrated without silencing either?
- When does exploration become unethical because the information can be obtained offline?
- How should epsilon or lane capacity adapt to regime change?
- Does preserving negative evidence measurably reduce duplicated experimentation?
- Which decisions improve when option value is made explicit?

## 17. Open issues

1. Define a credible measure of epistemic uncertainty for each application; softmax scores are not uncertainty estimates.
2. Decide whether portfolio allocation should be fixed, adaptive, or regime-dependent.
3. Define behavioral descriptors for novelty without encoding irrelevant diversity.
4. Separate risk borne by the sponsor from risk imposed on users, operators, citizens, or the environment.
5. Determine how long negative evidence remains applicable after a system or regime changes.
6. Design incentives that reward credible learning rather than positive results.
7. Determine the minimum evidence burden for low-cost wildcards without bureaucratizing ideation.
8. Establish how contradictory human intuitions are represented and tested.
9. Formalize the interface between recommender output and accountable judgment.

## 18. References and sources to work

These sources ground different components of the direction. Their inclusion does not imply that EGEP as a complete governance method is already empirically validated.

### Reinforcement learning and exploration foundations

- Sutton, R. S., & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*, 2nd ed. [Author-hosted book](http://incompleteideas.net/book/RLbook2020.pdf).
- Auer, P., Cesa-Bianchi, N., & Fischer, P. (2002). “Finite-time Analysis of the Multiarmed Bandit Problem.” *Machine Learning*, 47, 235–256. [DOI](https://doi.org/10.1023/A:1013689704352).
- Russo, D., Van Roy, B., Kazerouni, A., Osband, I., & Wen, Z. (2018). “A Tutorial on Thompson Sampling.” *Foundations and Trends in Machine Learning*. [arXiv](https://arxiv.org/abs/1707.02038).
- Osband, I., Blundell, C., Pritzel, A., & Van Roy, B. (2016). “Deep Exploration via Bootstrapped DQN.” NeurIPS. [Paper](https://proceedings.neurips.cc/paper/2016/hash/8d8818c8e140c64c743113f563cf750f-Abstract.html).
- Osband, I., Aslanides, J., & Cassirer, A. (2018). “Randomized Prior Functions for Deep Reinforcement Learning.” NeurIPS. [Paper](https://proceedings.neurips.cc/paper/2018/hash/5a7b238ba0f6502e5d6be14424b20ded-Abstract.html).

### Curiosity, information, and active exploration

- Pathak, D., Agrawal, P., Efros, A. A., & Darrell, T. (2017). “Curiosity-driven Exploration by Self-supervised Prediction.” ICML. [PMLR](https://proceedings.mlr.press/v70/pathak17a.html).
- Burda, Y., Edwards, H., Storkey, A., & Klimov, O. (2019). “Exploration by Random Network Distillation.” ICLR. [OpenReview](https://openreview.net/forum?id=H1lJJnR5Ym).
- Shyam, P., Jaśkowski, W., & Gomez, F. (2019). “Model-Based Active Exploration.” ICML. [PMLR](https://proceedings.mlr.press/v97/shyam19a.html).
- Sontakke, S. A., Mehrjou, A., Itti, L., & Schölkopf, B. (2021). “Causal Curiosity: RL Agents Discovering Self-supervised Experiments for Causal Representation Learning.” ICML. [PMLR](https://proceedings.mlr.press/v139/sontakke21a.html).
- Mavor-Parker, A. N., et al. (2022). “How to Stay Curious while avoiding Noisy TVs using Aleatoric Uncertainty Estimation.” ICML. [PMLR](https://proceedings.mlr.press/v162/mavor-parker22a.html).

### Novelty and Quality-Diversity

- Lehman, J., & Stanley, K. O. (2011). “Abandoning Objectives: Evolution through the Search for Novelty Alone.” *Evolutionary Computation*, 19(2), 189–223. [DOI](https://doi.org/10.1162/EVCO_a_00025).
- Mouret, J.-B., & Clune, J. (2015). “Illuminating Search Spaces by Mapping Elites.” [arXiv](https://arxiv.org/abs/1504.04909).
- Pugh, J. K., Soros, L. B., & Stanley, K. O. (2016). “Quality Diversity: A New Frontier for Evolutionary Computation.” *Frontiers in Robotics and AI*. [Article](https://doi.org/10.3389/frobt.2016.00040).
- Fontaine, M. C., et al. (2021). “Optimizing Distributions of Solutions in Black-Box Problems.” ICML. [PMLR](https://proceedings.mlr.press/v139/fontaine21a.html).

### Human-guided and preference-based learning

- Christiano, P. F., et al. (2017). “Deep Reinforcement Learning from Human Preferences.” NeurIPS. [Paper](https://proceedings.neurips.cc/paper/2017/hash/d5e2c0adad503c91f91df240d0cd4e49-Abstract.html).
- Wang, F., et al. (2018). “Intervention Aided Reinforcement Learning for Safe and Practical Policy Optimization in Navigation.” CoRL. [PMLR](https://proceedings.mlr.press/v87/wang18a.html).
- Chen, X., et al. (2022). “Human-in-the-loop Preference-based Reinforcement Learning with an Application to Robot-Assisted Navigation.” ICML. [PMLR](https://proceedings.mlr.press/v162/chen22ag.html).
- Hoque, R., et al. (2022). “ThriftyDAgger: Budget-Aware Novelty and Risk Gating for Interactive Imitation Learning.” CoRL. [PMLR](https://proceedings.mlr.press/v164/hoque22a.html).

### Bayesian optimization, safe experimentation, and sequential investment

- Frazier, P. I. (2018). “A Tutorial on Bayesian Optimization.” [arXiv](https://arxiv.org/abs/1807.02811).
- Snoek, J., Larochelle, H., & Adams, R. P. (2012). “Practical Bayesian Optimization of Machine Learning Algorithms.” NeurIPS. [Paper](https://proceedings.neurips.cc/paper/2012/hash/05311655a15b75fab86956663e1819cd-Abstract.html).
- Sui, Y., Gotovos, A., Burdick, J., & Krause, A. (2015). “Safe Exploration for Optimization with Gaussian Processes.” ICML. [PMLR](https://proceedings.mlr.press/v37/sui15.html).
- Letham, B., et al. (2019). “Bayesian Optimization for Policy Search via Online-Offline Experimentation.” *JMLR*, 20. [Paper](https://www.jmlr.org/papers/v20/18-225.html).
- March, J. G. (1991). “Exploration and Exploitation in Organizational Learning.” *Organization Science*, 2(1), 71–87. [DOI](https://doi.org/10.1287/orsc.2.1.71).
- McGrath, R. G. (1997). “A Real Options Logic for Initiating Technology Positioning Investments.” *Academy of Management Review*, 22(4), 974–996. [DOI](https://doi.org/10.5465/amr.1997.9711022113).

### Safety and constrained decision-making

- Altman, E. (1999). *Constrained Markov Decision Processes*. Chapman & Hall/CRC. [INRIA-hosted PDF](https://www-sop.inria.fr/members/Eitan.Altman/TEMP/h.pdf).
- García, J., & Fernández, F. (2015). “A Comprehensive Survey on Safe Reinforcement Learning.” *JMLR*, 16. [Paper](https://www.jmlr.org/papers/v16/garcia15a.html).

## 19. Short doctrine

The current best-known option deserves most, but not all, of the capacity. Uncertainty, contradiction, novelty, diverse behavior, and human intuition are legitimate reasons to experiment. A deliberately protected pocket of “productive madness” can create disproportionate value precisely because it is not forced to look optimal inside yesterday's model.

The price of that freedom is disciplined containment:

\[
\boxed{
\text{freedom to propose}
+\text{small reversible bets}
+\text{hard safety bounds}
+\text{mandatory learning}
+\text{accountable judgment}
}
\]

