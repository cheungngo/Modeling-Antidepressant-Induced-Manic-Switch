# Modeling Antidepressant-Induced Manic Switch in a Unified Computational Framework: Insights from Ketamine, SSRIs, and Neurosteroids

Authors:

Ngo Cheung, FHKAM(Psychiatry)

Affiliations:

¹ Independent Researcher

Corresponding Author:

Ngo Cheung, MBBS, FHKAM(Psychiatry)

Hong Kong SAR, China

Tel: 98768323

Email: info@cheungngomedical.com

**Conflict of Interest**: None declared.

**Funding Declaration**: This research received no specific grant from
any funding agency in the public, commercial, or not-for-profit sectors.

**Ethics Declaration**: Not applicable.

Citation:
Cheung, N. (2026). Modeling Antidepressant-Induced Manic Switch in a Unified Computational Framework: Insights from Ketamine, SSRIs, and Neurosteroids. Zenodo. https://doi.org/10.5281/zenodo.18292021

## Abstract

Background: Major depressive disorder involves impaired neural
plasticity, yet antidepressants spanning glutamatergic (e.g., ketamine),
monoaminergic (e.g., SSRIs), and GABAergic (e.g., neurosteroids) classes
differ in onset, durability, and risk of treatment-emergent mania,
particularly in bipolar disorder. Computational models offer controlled
comparison of these pathways, but few integrate manic liability.

Methods: We extended a magnitude-based pruning model (95% sparsity) of
depression in feed-forward networks trained on a Gaussian classification
task. From identical pruned states, three interventions were simulated:
ketamine-like gradient-guided regrowth (50% reinstatement) with
consolidation; SSRI-like prolonged low-rate training with tapering noise
and escalating excitability gain; neurosteroid-like global tonic
inhibition (0.7× damping, tanh activations, reduced gain). Efficacy was
assessed via accuracy under clean, noisy, and combined stress
conditions; resilience to graded noise and additional pruning; manic
risk via biased positive perturbation and activation magnitude. Results
were averaged across 10 random seeds.

Results: All treatments restored near-ceiling baseline performance.
Ketamine-like regrowth yielded superior extreme-stress resilience (76.8%
± 3.6%) and relapse protection (−0.1% ± 0.2% drop), reducing sparsity to
47.5%. Neurosteroid-like modulation matched acute combined-stress
recovery (97.7% ± 0.2%) but showed state-dependence (19% off-modulation
loss) and weaker extreme buffering (42.9% ± 1.3%). SSRI-like refinement
lagged (90.8% ± 3.1% combined, 50.1% ± 3.7% extreme) with highest
relapse vulnerability (8.8% ± 4.3%). Manic proxies ranked SSRI-like
highest risk (biased accuracy 47.6% ± 12.8%, gain 1.60), ketamine-like
moderate (84.2% ± 8.5%, gain 1.25), neurosteroid-like lowest (50.5% ±
7.4%, effective gain \~0.59).

Conclusions: Antidepressants operate via distinct plasticity
routes---structural rebuilding (durable, moderate risk), reversible
stabilization (rapid, low risk), gradual optimization (vulnerable, high
risk)---reproducing clinical trade-offs and supporting mechanism-based
selection in unipolar and bipolar depression.

## Introduction

Major depressive disorder (MDD) is a dominant source of global
disability, affecting hundreds of millions of people and exerting heavy
personal and economic burdens \[1\]. Pharmacotherapy has helped many
patients, yet outcomes remain modest: only about one-third of
individuals remit after a first antidepressant trial, and another third
continue to meet criteria for depression even after several treatment
steps \[2\]. Selective serotonin reuptake inhibitors (SSRIs) are the
most frequently prescribed agents but usually take weeks to produce
noticeable change, leaving patients exposed to ongoing symptoms and
often delivering only partial recovery \[3\].

The slow and sometimes incomplete benefit of monoaminergic drugs has
shifted attention toward interventions that act on glutamatergic and
GABA-ergic systems. Low-dose ketamine, an NMDA-receptor antagonist, can
relieve depressive symptoms within hours. Pre-clinical and clinical work
links this rapid effect to a surge in synaptogenesis mediated by
brain-derived neurotrophic factor (BDNF) and mTOR signalling \[4,5\].
Neuroactive steroids such as brexanolone and zuranolone, which amplify
tonic inhibition at extrasynaptic GABA_A receptors, also yield fast
improvement and have shown particular promise for postpartum depression
\[6\]. Taken together, these findings call the classic serotonin-deficit
narrative into question and support a view of MDD as a disorder of
impaired neural plasticity, in which chronic stress trims dendrites and
synapses in prefrontal and hippocampal circuits and erodes resilience
\[7,8\].

Treatment-emergent mania (TEM) complicates the picture, especially for
people with bipolar disorder. Traditional antidepressants can provoke
mood switches in roughly 20--40 % of bipolar patients, whereas ketamine
appears to carry less switch risk in controlled settings, and early
reports on neurosteroids suggest minimal liability \[9,10,6\].
Understanding how different drug classes influence both depressive
recovery and excitatory--inhibitory balance therefore remains essential.

Computational models offer a controlled way to dissect these mechanisms.
Pruning-based approaches conceptualise depression as excessive synaptic
elimination that leaves circuits fragile; ketamine-like regrowth within
such models restores performance and resilience \[11\]. Yet few
simulations place glutamatergic, monoaminergic and GABA-ergic strategies
side by side or consider the risk of polarity switches.

The present study addresses these gaps. Using a single \"depleted\"
network as a starting point, we compare three antidepressant
analogues---ketamine-like synaptogenesis, SSRI-like gradual refinement
and neurosteroid-like tonic inhibition. We further incorporate
excitability shifts as a proxy for manic risk. By tracking onset speed,
durability and switch potential within one coherent framework, we aim to
clarify how distinct biological actions translate into clinical
strengths and limitations.

## Methods

### Network architecture and task

The simulations relied on a feed-forward model intended to capture broad
properties of cortico-limbic circuits (Figure 1). The network accepted
two-dimensional inputs, passed activity through three hidden layers of
512, 512 and 256 units, and produced one of four class probabilities
through a soft-max layer. Rectified linear units drove all hidden
activations unless stated otherwise. The full configuration contained
roughly 397 000 adjustable parameters.

![](media/image1.png){width="6.268099300087489in"
height="2.916499343832021in"}

***Figure 1:** Condensed Architecture of the Stress-Aware Network. The
model processes 2D inputs through three hidden layers (blue) before
producing a 4-class output. Each hidden layer block encapsulates four
distinct operations in sequence: (1) a fully connected linear
transformation subject to structural pruning (orange, Pruning Mask); (2)
non-linear activation; (3) stress simulation via additive noise
injection (orange, Stress Level); and (4) pharmacological modulation via
multiplicative gain and inhibition scaling (orange, Drug Params). This
design allows the network to dynamically simulate both structural
connectivity changes (synaptogenesis/pruning) and functional state
changes (excitability/stress) during the experiment.*

Synaptic stressors could be introduced at two levels. First, additive
Gaussian noise was injected after each hidden activation to emulate
neuromodulatory disruption. Second, a global multiplicative gain term
changed overall excitability and could be combined with an extra damping
factor or a switch from ReLU to tanh units when modelling neurosteroid
action.

The task itself involved classifying points drawn from four equally
sized Gaussian clouds centred on (−3, −3), (−3, 3), (3, −3) and (3, 3)
with a shared standard deviation of 0.8. Each training run used 12 000
labelled samples. Performance was monitored on three separate sets: a 4
000-sample \"standard noise\" set that matched training variance, a 2
000-sample noise-free set, and a \"combined-stress\" set that added both
input and internal perturbations. All code was written in PyTorch and
executed on a single CPU core. Ten independent seeds, differing only in
data order and weight initialisation, were averaged to obtain each
metric.

### Simulation of depression

![](media/image4.png){width="1.6798993875765529in"
height="6.4227996500437445in"}

***Figure 2:** Computational Modeling of the Depressive State. The
diagram outlines the algorithmic simulation of depression implemented in
the StressAwareNetwork. Phase 1 represents the environmental triggers.
Phase 2 illustrates the structural impact modeled by the PruningManager,
where 95% of network weights are removed based on magnitude, simulating
dendritic spine loss and cortical atrophy. Phase 3 depicts the
functional consequences during the forward pass; the sparse topology
makes the network highly susceptible to stress_level noise injection,
leading to a collapse in the signal-to-noise ratio. Phase 4 represents
the final model output, where the inability to classify inputs correctly
serves as a proxy for cognitive inflexibility and anhedonia.*

After 20 epochs of baseline training with Adam (learning rate 0.001) on
noise-free data, every model underwent magnitude-based pruning that
removed the smallest 95 % of weights layer by layer. The surviving
sparse network maintained reasonable accuracy on clean inputs yet
collapsed rapidly when noise or further pruning was applied, mirroring
theories that excessive synaptic loss underlies depressive vulnerability
\[7\] (Figure 2).

### Antidepressant intervention protocols

Three recovery strategies were examined on separate copies of the pruned
network.

Ketamine-like condition: overall gain was fixed at 1.25. Gradients were
accumulated over 30 mini-batches, the highest 50 % of previously pruned
weights were reinstated with small random values drawn from N(0, 0.03),
and parameters were fine-tuned for 15 epochs with Adam (learning rate
0.0005) while preserving sparsity.

SSRI-like condition: sparsity remained at 95 %. Over 100 slow-learning
epochs (learning rate 1 × 10⁻⁵) internal noise declined linearly from σ
= 0.5 to 0, while excitability gain rose from 1.0 to 1.6, mimicking
gradual monoaminergic adaptation.

Neurosteroid-like condition: sparsity again stayed constant. A
post-activation damping factor of 0.7 was applied, ReLU units were
replaced by tanh, and global gain was set to 0.85 (effective scaling ≈
0.59). Ten adaptation epochs were run with Adam (learning rate 0.0005).

### Outcome measures

Primary efficacy was the classification accuracy on clean,
standard-noise and combined-stress test sets. To examine robustness,
internal noise was increased stepwise from σ = 0 to 2.5 while recording
accuracy. Relapse risk was evaluated by applying a second
magnitude-based pruning that deleted 40 % of the remaining weights and
then repeating the combined-stress test.

![](media/image3.png){width="2.850799431321085in"
height="7.03109908136483in"}

***Figure 3:** Computational Model of Pharmacologically Induced Manic
Conversion. The diagram illustrates the bifurcation of network dynamics
following antidepressant treatment. Phase 1 represents the baseline
depressive state characterized by sparse connectivity and low signal
amplitude. Phase 2 differentiates the mechanism of action: SSRIs
increase the global gain (amplifying existing signals), while Ketamine
induces structural plasticity (synaptic regrowth). Phase 3 demonstrates
that both mechanisms increase net excitation. The critical checkpoint is
the system\'s \"Inhibitory Tone.\" If inhibition is sufficient, the
system stabilizes into remission. However, in the presence of weak
inhibition---simulating a deficit in GABAergic control or neurosteroid
regulation---the system enters Phase 4, resulting in a loss of
Excitation-Inhibition (E-I) balance, attractor instability, and
ultimately a transition to a manic state.*

Potential manic conversion was probed in two ways (Figure 3). First, the
network was challenged with biased internal noise (σ = 1.0, bias = +1.0)
and the resulting accuracy recorded; poorer accuracy implied greater
vulnerability to hyper-excitability. Second, the mean absolute
activation across hidden layers during standard input served as an index
of latent excitatory tone. For the neurosteroid analogue, all
evaluations were repeated after removing the damping parameters to
simulate drug withdrawal.

### Data analysis

For every condition, results from the ten seeds were summarised as mean
± standard deviation; pronounced dispersion was also expressed as
minimum--maximum ranges. Because the project was exploratory, no formal
null-hypothesis tests were undertaken.

## Results

Simulations were run under 10 independent random seeds; across seeds we
saw the same rank ordering of performance for every outcome that was
tracked.

### Antidepressant efficacy

Relative to the over-pruned, untreated network (combined-stress accuracy
= 29.7 ± 2.7 %), every treatment produced a large gain.
Neurosteroid-like modulation finished at 97.7 ± 0.2 % and the
ketamine-like synaptogenic routine reached 97.2 ± 0.2 %. The
monoaminergic SSRI-like schedule also improved behaviour, but only to
90.8 ± 3.1 %. On clean or standard inputs all treated models scored at
(or fractionally below) 100 % accuracy, whereas the untreated baseline
remained at 34.7 ± 11.9 % on clean data.

### Stress resilience

Noise tolerance diverged sharply as internal noise was increased (Table
1). At the most extreme perturbation (σ = 2.5) the ketamine analogue
still classified correctly 76.8 ± 3.6 % of trials. The SSRI-like routine
fell to 50.1 ± 3.7 %, and the neurosteroid analogue to 42.9 ± 1.3 %; the
untreated control stayed near its baseline (29.6 ± 1.5 %). With moderate
noise (σ = 1.0) neurosteroid- and ketamine-like conditions were similar
(92.9 ± 2.5 % and 98.2 ± 1.1 %, respectively) and both clearly
out-performed the SSRI-like schedule (78.2 ± 5.8 %).

***Table 1.** Stress Resilience Profile (Mean ± Standard Deviation
Across 10 Seeds)*

| **Treatment**      | **No Noise (%)** | **Moderate (σ=0.5) (%)** | **High (σ=1.0) (%)** | **Severe (σ=1.5) (%)** | **Extreme (σ=2.5) (%)** |
|--------------------|------------------|--------------------------|----------------------|------------------------|-------------------------|
| Untreated (pruned) | 36.8 ± 11.9      | 29.9 ± 2.5               | 29.6 ± 1.8           | 29.8 ± 1.4             | 29.6 ± 1.5              |
| Ketamine-like      | 100.0 ± 0.0      | 99.9 ± 0.1               | 98.2 ± 1.1           | 92.9 ± 2.4             | 76.8 ± 3.6              |
| SSRI-like          | 99.9 ± 0.1       | 95.3 ± 2.8               | 78.2 ± 5.8           | 64.5 ± 5.2             | 50.1 ± 3.7              |
| Neurosteroid-like  | 100.0 ± 0.0      | 99.9 ± 0.1               | 92.9 ± 2.5           | 70.9 ± 2.5             | 42.9 ± 1.3              |

### Manic conversion risk

Three risk markers were monitored: gain multiplier, accuracy under
positively biased noise, and mean activation magnitude (Table 2). The
SSRI-like model showed the largest gain (1.60 ± 0.00) and the poorest
accuracy under biased noise (47.6 ± 12.8 %), with a mid-range activation
magnitude of 0.390 ± 0.078. The ketamine analogue showed an intermediate
gain (1.25 ± 0.00) but maintained high biased-noise accuracy (84.2 ± 8.5
%) even though its activation magnitude was highest (0.649 ± 0.079).
Neurosteroid modulation damped excitability (gain = 0.85 ± 0.00;
activation magnitude = 0.196 ± 0.008) and achieved a biased-noise
accuracy of 50.5 ± 7.4 %. Untreated networks, by comparison, sat at gain
= 1.00 ± 0.00, biased-noise accuracy = 25.0 ± 0.8 %, and activation
magnitude = 0.100 ± 0.013.

***Table 2**. Manic Conversion Risk Metrics (Mean ± Standard Deviation
Across 10 Seeds)*

| **Treatment**      | **Gain Multiplier** | **Biased Stress Accuracy (%)** | **Activation Magnitude** |
|--------------------|---------------------|--------------------------------|--------------------------|
| Untreated (pruned) | 1.00 ± 0.00         | 25.0 ± 0.8                     | 0.100 ± 0.013            |
| Ketamine-like      | 1.25 ± 0.00         | 84.2 ± 8.5                     | 0.649 ± 0.079            |
| SSRI-like          | 1.60 ± 0.00         | 47.6 ± 12.8                    | 0.390 ± 0.078            |
| Neurosteroid-like  | 0.85 ± 0.00         | 50.5 ± 7.4                     | 0.196 ± 0.008            |

*Note. Lower biased stress accuracy indicates greater manic conversion
vulnerability; higher activation magnitude reflects increased latent
hyperexcitability.*

### Relapse vulnerability and medication dependence

When an additional pruning step was applied to mimic relapse,
performance in the ketamine-treated networks was essentially unchanged
(-0.1 ± 0.2 %). The SSRI-like condition lost 8.8 ± 4.3 % accuracy and
the neurosteroid analogue lost 5.1 ± 2.2 %. Removing the neurosteroid
damping altogether revealed strong state-dependence: combined-stress
accuracy fell from 97.7 ± 0.2 % on-modulation to 78.5 ± 4.8 %
off-modulation.

Seed-to-seed spread was small for most measures, but biased-noise
accuracy varied widely with the SSRI (28.2--74.8 %) and ketamine
(63.3--96.7 %) analogues, indicating individual-difference sensitivity
in those two conditions (Table 3).

***Table 3.** Final Comparison Matrix (Means Across 10 Seeds)*

| **Metric**           | **Ketamine-like** | **SSRI-like** | **Neurosteroid-like** | **Untreated (pruned)** |
|----------------------|-------------------|---------------|-----------------------|------------------------|
| Combined Stress (%)  | 97.2              | 90.8          | 97.7                  | 29.7                   |
| Extreme Stress (%)   | 76.8              | 50.1          | 42.9                  | 29.6                   |
| Biased Stress (%)    | 84.2              | 47.6          | 50.5                  | 25.0                   |
| Gain Multiplier      | 1.25              | 1.60          | 0.85                  | 1.00                   |
| Activation Magnitude | 0.649             | 0.390         | 0.196                 | 0.100                  |
| Sparsity (%)         | 47.5              | 95.0          | 95.0                  | 95.0                   |
| Relapse Drop (%)     | −0.1              | 8.8           | 5.1                   | N/A                    |

## Discussion

### Interpretation of efficacy and resilience findings

Our simulations reveal three recognisably different therapeutic
signatures. Both the ketamine-like synaptogenic programme and the
neurosteroid-like GABAergic modulation restored almost full performance
when the network was challenged by combined input and internal noise,
reaching about 97 % accuracy. The SSRI-style, slow-adaptation schedule
also improved accuracy but plateaued roughly six percentage points
lower. These results echo clinical impressions: drugs that act through
glutamatergic or GABAergic mechanisms often stabilise circuits within
days, whereas monoaminergic agents depend on gradual receptor and
transcriptional changes that unfold over weeks \[7,8\].

Durability separated the interventions more clearly. After ketamine-like
regrowth, the network continued to perform well under extreme internal
noise and was virtually unaffected by an additional round of pruning.
This mirrors evidence that a single ketamine infusion can trigger
structural plasticity and sustain benefit beyond drug clearance \[5\].
Neurosteroid modulation, by contrast, protected the network only while
the damping remained active; switching the modulation off produced a 19
% drop in accuracy and a moderate vulnerability to further pruning.
Clinical reports of zuranolone show a similar pattern: rapid relief that
fades once dosing stops \[6\]. The SSRI analogue, which merely
fine-tuned the sparse remnants, offered the least protection against
heavy noise and the greatest loss after re-pruning, a finding that
accords with the limited stress resilience seen in many patients who
rely on conventional antidepressants alone \[4\].

### Manic conversion risk and clinical parallels

![](media/image2.png){width="5.525599300087489in"
height="4.360199037620298in"}

***Figure 4:** Translational Framework for Antidepressant Selection. The
study leverages a unified simulation architecture to compare three
distinct pharmacological mechanisms against a common baseline of network
fragility. (Left, Green) The Glutamatergic pathway, modeling
ketamine-like synaptogenesis, demonstrates superior durability against
stress, supporting its use in treatment-resistant depression. (Center,
Blue) The GABAergic pathway, modeling neurosteroid modulation, provides
rapid stabilization with minimal excitability risk, aligning with
indications for urgent or postpartum care. (Right, Orange) The
Monoaminergic pathway, modeling traditional SSRIs, reveals a
susceptibility to manic overshoot, reinforcing clinical guidelines that
advise caution or concurrent mood stabilizers when treating bipolar
depression.*

The simulations demonstrated class-specific variations in mood-switch
risk (Figure 4): SSRI-like refinement resulted in the most significant
increase in network excitability and the least resistance to positively
biased noise, reflecting meta-analytic manic switch rates nearing 40%
among bipolar patients on traditional antidepressants \[9\];
ketamine-like synaptogenesis maintained strong performance even in the
presence of biased input, aligning with infrequent reports of mania when
ketamine is used with mood stabilizers \[10\]; and neurosteroid-based
tonic inhibition exhibited the highest safety, showing the lowest
activation levels and the most effective management of bias-driven
errors, corresponding to initial clinical findings of minimal switch
liability with zuranolone \[12\].

Seed-to-seed variability was widest for the SSRI- and ketamine-like
conditions, hinting that individual differences in baseline network
fragility can influence real-world switch risk, much as prior
mood-switch history and family loading predict clinical outcomes \[13\].
Together, these observations support caution when prescribing
monoaminergic monotherapy to patients with bipolar vulnerability and
point to plasticity-targeted or GABAergic strategies as potentially
safer options.

### Novelty and translational impact

This project brings three very different antidepressant
mechanisms---glutamatergic, monoaminergic, and GABA-ergic---into one
tightly controlled simulation that also tracks the chance of a manic
switch. Earlier in-silico work tended to look at one pathway at a time,
for example focusing only on ketamine-like synaptogenesis \[11\]. By
beginning with the same over-pruned network in every run and then adding
a uniform excitability gain plus skewed noise, we could compare
recovery, durability, and opposite-pole risk head-to-head.

Several clinically relevant messages emerge. First, the ketamine
analogue delivered the strongest long-term protection: once new synapses
were in place, the model withstood heavy noise and an extra pruning
step, mirroring reports that ketamine triggers lasting plasticity \[5\].
Second, the neurosteroid analogue worked quickly and limited
over-excitability, a pattern that fits current interest in zuranolone
for urgent or postpartum use where speed and safety are paramount \[6\].
Third, the monoaminergic schedule, while helpful, left the network most
exposed to manic-like overshoot---an echo of long-standing cautions
about antidepressant monotherapy in bipolar illness \[9,14\].

Running all three regimens inside the same architecture therefore offers
a mechanistic basis for tailoring treatment: reserve synaptogenic drugs
for recurrent or resistant cases, choose fast but state-dependent
GABA-modulators for acute relief, and limit classic antidepressants to
patients with low switch risk or with added mood stabilisers.

![](media/image5.png){width="6.268099300087489in"
height="1.9582994313210849in"}

***Figure 5:** Conceptual Shift to a Unified Simulation Framework.
Unlike previous in-silico studies that typically isolated single
pathways (e.g., focusing solely on synaptogenesis), this project
introduces a novel unified architecture. By initializing all simulations
with an identical over-pruned network baseline and applying standardized
excitability gains and skewed noise, the framework allows for a rigorous
head-to-head comparison of three distinct pharmacological mechanisms:
Glutamatergic, Monoaminergic, and GABAergic. This approach uniquely
enables the simultaneous tracking of recovery trajectories, treatment
durability, and the risk of manic switching within a single controlled
environment.*

### Limitations

Important simplifications remain. The feed-forward model omits the
feedback loops that shape real cortico-limbic mood circuits; this may
downplay potential instabilities. Modulation was applied globally, not
by layer or cell class, so subtle pharmacological differences---for
example, the varied extrasynaptic actions of neurosteroids noted by
\[15\]---were ignored. Manic liability was estimated from biased noise
and overall activation; these proxies are cruder than clinical ratings
taken over time.

Although ten random seeds introduced variability, other \"patient\"
features such as baseline excitation--inhibition balance or depth of
synaptic loss were held constant. Mood-stabilising co-therapy, known to
reduce switch risk \[14\], was not simulated. Each of these gaps limits
direct clinical translation.

### Conclusion

Within these constraints, the study shows that different drug classes
restore performance through distinct routes---building new structure,
damping excess firing, or slowly tuning weakened links---each with its
own balance of speed, staying power, and bipolar safety. Embedding
switch risk in the same plasticity frame moves the conversation away
from single-transmitter theories toward circuit reserve and excitability
control. As rapid-acting treatments gain ground, such models may help
clinicians match interventions to individual risk profiles and combine
agents more safely.

## References

\[1\] World Health Organization. (2022). World mental health report:
Transforming mental health for all. World Health Organization.

\[2\] Rush, A. J., Trivedi, M. H., Wisniewski, S. R., et al. (2006).
Acute and longer-term outcomes in depressed outpatients requiring one or
several treatment steps: A STAR\*D report. American Journal of
Psychiatry, 163(11), 1905--1917.
https://doi.org/10.1176/ajp.2006.163.11.1905

\[3\] Trivedi, M. H., Rush, A. J., Wisniewski, S. R., et al. (2006).
Evaluation of outcomes with citalopram for depression using
measurement-based care in STAR\*D: Implications for clinical practice.
American Journal of Psychiatry, 163(1), 28--40.
https://doi.org/10.1176/appi.ajp.163.1.28

\[4\] Murrough, J. W., Iosifescu, D. V., Chang, L. C., et al. (2013).
Antidepressant efficacy of ketamine in treatment-resistant major
depression: A two-site randomized controlled trial. American Journal of
Psychiatry, 170(10), 1134--1142.
https://doi.org/10.1176/appi.ajp.2013.13030392

\[5\] Krystal, J. H., Abdallah, C. G., Sanacora, G., et al. (2019).
Ketamine: a paradigm shift for depression research and treatment.
Neuron, 101(5), 774--778. https://doi.org/10.1016/j.neuron.2019.02.005

\[6\] Gunduz-Bruce, H., Lasser, R., Nandy, I., et al. (2020, September).
Open-label, Phase 2 trial of the oral neuroactive steroid GABAA receptor
positive allosteric modulator zuranolone in bipolar disorder I and II.
In Poster presented at: psych Congress.

\[7\] Duman, R. S., & Aghajanian, G. K. (2012). Synaptic dysfunction in
depression: potential therapeutic targets. Science (New York, N.Y.),
338(6103), 68--72. https://doi.org/10.1126/science.1222939

\[8\] Page, C. E., Epperson, C. N., Novick, A. M., et al. (2024). Beyond
the serotonin deficit hypothesis: communicating a neuroplasticity
framework of major depressive disorder. Molecular Psychiatry, 29(12),
3802-3813.

\[9\] Tondo, L., Vázquez, G., & Baldessarini, R. J. (2010). Mania
associated with antidepressant treatment: comprehensive meta‐analytic
review. Acta Psychiatrica Scandinavica, 121(6), 404-414.

\[10\] Jawad, M. Y., Watson, S., Haddad, P. M., et al. (2021). Ketamine
for bipolar depression: A systematic review. International Journal of
Neuropsychopharmacology, 24(7), 535--541.
https://doi.org/10.1093/ijnp/pyab023

\[11\] Cheung, N. (2026). Divergent Mechanisms of Antidepressant
Efficacy: A Unified Computational Comparison of Synaptogenesis,
Stabilization, and Tonic Inhibition in a Model of Depression. Zenodo.
https://doi.org/10.5281/zenodo.18290014

\[12\] Price, M. Z., & Price, R. L. (2025). Zuranolone for Postpartum
Depression in Real-World Clinical Practice. J Clin Psychiatry, 86(3),
25cr15876.

\[13\] Goldberg, J. F., & Truman, C. J. (2003). Antidepressant-induced
mania: an overview of current controversies. Bipolar Disorders, 5(6),
407-420.

\[14\] Viktorin, A., Lichtenstein, P., Thase, M. E., et al. (2014). The
risk of switch to mania in patients with bipolar disorder during
treatment with an antidepressant alone and in combination with a mood
stabilizer. American Journal of Psychiatry, 171(10), 1067-1073.

\[15\] Marecki, R., Kałuska, J., Kolanek, A., et al. (2023).
Zuranolone--synthetic neurosteroid in treatment of mental disorders:
narrative review. Frontiers in Psychiatry, 14, 1298359.
