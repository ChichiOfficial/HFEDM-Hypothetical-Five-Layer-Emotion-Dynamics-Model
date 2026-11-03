# HFEDM: Hypothetical Five-Layer Emotion Dynamics Model

**A Five-Layer Emotion Dynamics System for Dialogue and Task Contexts**

**Aher (ChichiOfficial)**

## Abstract

If dialogue agents primarily rely on turn-by-turn language inference without explicitly maintaining a persistent emotional state, cross-turn consistency, state update rules, and module responsibilities require separate specification. With limited computational resources and under explicitly stated constraints, this paper proposes a computable emotion model with bounded psychological states, and explicitly decouples the emotion module from the memory module in terms of ownership and update authority.

The core contribution is the organization of the emotion space into a self-consistent five-layer dynamical model within a given system, the introduction of a moving restriction box \(\mathcal{C}(m)\), a bounded affective-style adaptation mechanism, and cross-module ownership boundaries. The deliverables include a fully computable mathematical model and specification, initial numerical settings for implementation, and interface contracts with external modules (particularly memory).

This work is currently positioned as a hypothetical theory of emotion dynamics: it has not yet been integrated with a complete memory theory, nor has it been validated as a model of human emotion.

## 1. Problem and Motivation

This paper focuses on dialogue and task agents that rely on turn-by-turn language inference but do not explicitly maintain a persistent emotional state. In the absence of independent state and update contracts, the system must address the following design issues:

- How to establish explicit, repeatable computational rules for emotional states;
- How to maintain state consistency across long dialogues;
- How to distinguish the update responsibilities of emotion, memory, and personality-related states;
- How to retain emotional history in finite summaries of the state while controlling the computational cost of the associated context.

Under limited computational resources and the requirement for long-term consistent behavior, emotional states should be computable and bounded rather than regenerated on every turn. This paper therefore proposes a five-layer emotion dynamics model that aims to:

1. Organize the emotion space into a self-consistent dynamical structure within a given system;
2. Constrain the short-term mood \(x\) via the moving restriction box \(\mathcal{C}(m)\);
3. Impose explicit bounds on affective-style adaptation;
4. Provide bounded adaptation processes for affective-style baselines and object-directed affective tendencies;
5. Decouple ownership and update authority from the memory module.

The present work is positioned as a hypothetical theory of emotion dynamics and mathematical specification that provides a computable model, initial settings for implementation, and interface contracts; integration with a complete memory theory and validation against human emotional responses remain unfinished.

## 2. Related Work and Design Choices

Psychology and affective computing provide reference constructs and computational frameworks; applying them to dialogue agents still requires explicit specification of states, update rules, parameter domains, and cross-module interfaces. This section explains the sources and trade-offs of the design adopted in this paper and contrasts it with two related frameworks.

### 2.1 Comparison with Existing Frameworks

**ALMA (A Layered Model of Affect)**  
ALMA divides affect into three layers—personality, mood, and emotion—combining discrete emotions with a PAD mood representation (Gebhard, 2005). Its EmotionEngine distinguishes multiple emotions and maps them into PAD space to influence mood. The principal differences from the present model are:

- Representation of proximal reactions \(e\), long-term background \(m\), and short-term mood \(x\) by a fixed ten-axis system rather than compression of mood into PAD coordinates;
- Introduction of the moving restriction box \(\mathcal{C}(m)\) so that the long-term background directly constrains the allowable range of short-term mood \(x\);
- Explicit separation of object-directed sentiments from a long-term background that is not directed at any particular object;
- Isolation of subjective mental fatigue as an auxiliary state;
- Application of a bounded, experience-gated mechanism to affective-style adaptation, together with ownership decoupling from the memory module.

**TESSERA Framework**  
TESSERA (Wrzus & Roberts, 2017) emphasizes how repeated situational experiences gradually influence longer-term personality traits through a series of processes. The present work uses it as a conceptual reference but imposes the following restrictions and operationalizations:

- Adaptation is limited to the affective-style baseline \(b\) rather than the full personality structure;
- Implementation through an experience-integration equation with a capacity limit, discard rules, and upper bounds on the integration rate;
- The adaptation process is not authorized to rewrite memory strategies or other module parameters.

### 2.2 Literature-Supported Constructs

- **Appraisal theory**: Emotions are related to appraisal differences in certainty, control, responsibility, and pleasantness (Smith & Ellsworth, 1985). This paper retains the idea of multi-field appraisal followed by mapping onto emotion axes, but the specific mapping functions are defined by this work.
- **Distinction between emotion and mood**: Emotion and mood can be distinguished along dimensions such as cause, duration, and directedness (Beedie et al., 2005). On this basis the paper distinguishes proximal reactions \(e\) from mood-related states and makes the additional design choice of dividing the latter into long-term background \(m\) and short-term mood \(x\).
- **Object-directed affective tendencies (sentiments)**: Research on evaluative conditioning supports the view that preferences toward objects can change through stimulus-pairing experiences (Hofmann et al., 2010). The positive/negative dual-channel representation, storage method, and biasing effect on ambiguous appraisal employed here are engineering operationalizations.
- **Fatigue and emotion regulation**: Mental fatigue can impair intentional emotion down-regulation under certain experimental conditions (Grillon et al., 2015). This paper represents subjective mental fatigue as an auxiliary state and treats it separately from current effort, motivation, and objective performance; both the state definition and its governing dynamical equation are engineering operationalizations.

The above literature supports the constructs and the direction of some relationships; it does not directly prove the equations, coefficients, or five-layer structure of this paper.

### 2.3 Principal Design Choices of This Paper

| Design | Choice | Rationale and Trade-off |
| ------ | ------ | ----------------------- |
| Emotion representation | Ten axes + fatigue auxiliary axis | Preserves important distinctions; avoids low-dimensional representations such as PAD or VA |
| Architecture | Five-layer dynamical system | Separates temperament anchoring, object-directed tendencies, long-term background, proximal reactions, and short-term mood |
| Short-term constraint | Moving restriction box \(\mathcal{C}(m)\) | Long-term background directly limits short-term mood \(x\); may attenuate the expression of strong proximal reactions in mood output |
| Personality adaptation | Bounded, experience-gated update of \(b\) | Permits only slow affective-style change and does not rewrite memory strategies |
| Relation to memory | Ownership and update authority decoupled | The emotion module does not modify memory content or alter retrieval or replay strategies |
| Time | Abstract time + optional interface to real elapsed time | No real-world clock is connected at this stage, but a linear mapping to real elapsed time is retained |

### 2.4 Directions Not Adopted or Deliberately Excluded

- Physiological arousal, hormones, sleep, or circadian rhythms are not modeled.
- Personality evolution is not extended to memory strategies, tool permissions, or other module parameters.
- The intensity of a single emotional response is not treated as new memory evidence or as a personality-learning quota.
- The ten axes are not claimed to constitute a complete human emotion space, nor is calibration for cross-cultural equivalence performed.
- Clinical psychological disorders, extreme moods, or unusual emotional changes are not included in the initial theoretical conception.

These choices keep the model computable and its boundaries clear within the intended scope, while limiting its ability to explain the full range of human emotion and long-term personality. Unless otherwise noted, the equations and parameters in subsequent sections are original designs or engineering extensions based on the constructs above.

## 3. Model Overview and Five-Layer Architecture

Emotional state is organized into five functional layers, each with its own time scale and responsibilities. Fatigue is an auxiliary state of L5 and does not constitute an additional functional layer; the memory module is an external cooperating module that interacts with the present model through explicit interfaces.

### 3.1 Five-Layer Structure

| Layer | Name | Principal States | Time Scale | Core Responsibility |
| --- | --- | --- | --- | --- |
| L1 | Temperament anchoring and affective-style adaptation | Fixed \(b_0, g, \theta_{\mathrm{reg}}\); slowly varying \(b\) and summaries \(Q_E, P^E\) | Slowest | Supplies stable baselines for individual differences and permits limited, slow experience-based adaptation |
| L2 | Object-directed sentiments | \(s_o^+, s_o^-\) | Slow | Affective tendencies toward specific people, groups, things, or activities |
| L3 | Long-term affective background | \(m\) | Slow | Integrates cross-event background and determines the allowable region of short-term mood |
| L4 | Proximal event reactions | \(e\) | Fast | Proximal reactions to new stimuli, reappraisal, ongoing situations, and recall |
| L5 | Short-term mood and regulatory load | \(w, x, f\) | Short to medium | Constrained present mood; subjective fatigue and recovery |

Key relations may be simplified as:

\[
s_o \to \text{appraisal} \to e \to m, \quad b \to m, \quad m \to e
\]

\[
(e,m) \to w, \quad (w,m) \to x, \quad \text{activity, effort and recovery opportunity} \to f
\]

Short-term mood \(x\) must lie inside the moving restriction box \(\mathcal{C}(m)\) determined by the long-term background; the definition appears in Section 4.3.

### 3.2 Interactions Between Layers (Conceptual)

- The baseline \(b\) of L1 influences the long-term background \(m\) of L3.
- Sentiments of L2 may influence appraisal when evidence is ambiguous, thereby affecting L4.
- Proximal reactions \(e\) of L4 and \(m\) of L3 jointly drive the mood \(x\) of L5.
- Eligible experience may feed back slowly to L1, updating the affective baseline \(b\), without rewriting memory.
- Subjective fatigue \(f\) influences regulatory efficacy and serves as the eleventh auxiliary output.

### 3.3 Design Intent

The five-layer division is intended to satisfy the following requirements simultaneously:

1. **Separation of time scales**: Prevent short-term fluctuations from directly rewriting long-term tendencies.
2. **State boundedness**: Restrict the reachable range of short-term mood via \(\mathcal{C}(m)\).
3. **Clear responsibilities**: Object-directed tendencies, background state, proximal reactions, and output mood are separated from one another.
4. **Decoupling from external modules**: Personality adaptation acts only on affective style and does not obtain write authority over memory strategies.

Subsequent sections summarize the core equations, event-update semantics, and interface contracts with the memory module.

## 4. Core Mechanisms and Formalization

This section summarizes the principal mathematical definitions of the five-layer architecture, focusing on how states are defined and updated, which quantities are constrained, and the boundaries between mechanisms. Unless otherwise noted, the specific functional forms, coefficients, and update rules are original designs.

Continuous evolution uses abstract time \(\tau\ge0\) as the independent variable; an overdot denotes differentiation with respect to \(\tau\). Abstract time is not dialogue turns, message counts, or token counts. The linear interface with real elapsed time \(t\) is \(\Delta\tau=\Delta t/T_{\mathrm{ref}}\), where \(T_{\mathrm{ref}}>0\) remains to be calibrated.

### 4.1 Emotion Space and Final Output

The fixed axis order is \(\mathcal{I} = (J, I, C, R, F, A, S, D, H, G)\):

- \(J\) Joy, \(I\) Interest, \(C\) Concern/Sympathy, \(R\) Relief
- \(F\) Fear/Anxiety, \(A\) Anger, \(S\) Sadness, \(D\) Disgust
- \(H\) Shame, \(G\) Guilt

Plus the auxiliary axis \(\mathrm{Fat}\) (subjective mental fatigue).

The final output consists of eleven values:

\[
y = (y_J, y_I, y_C, y_R, y_F, y_A, y_S, y_D, y_H, y_G, y_{\mathrm{Fat}})
\]

All axes take values in \([0,1]\); they are not required to sum to 1 and are not assumed to be independent. Fatigue is not produced by appraisal projection but is computed by a dedicated state equation driven by activity engagement and recovery processes; it may still couple with emotion dynamics through regulation. The ten-axis design aims to preserve distinctions (fear vs. anger, shame vs. guilt, joy vs. relief) that are easily confounded in low-dimensional representations, while acknowledging that it may not constitute a complete human emotion space.

### 4.2 Appraisal and Emotion Projection

An event first undergoes multi-field appraisal (gain/harm, threat, control, responsibility, loss, etc.) and is then mapped to a reaction target \(r \in [0,1]^{10}\) before temperament gain is applied. Event reliability and relevance act on effective dose separately and are not mixed into the semantics of the individual emotion axes. To distinguish it from real time, the threat level is denoted \(t_{\mathrm{thr}}\) below. Representative mappings include:

\[
\begin{aligned}
r_J &= g^+\left(1 - \frac{t_{\mathrm{thr}}}{2}\right), \\
r_F &= t_{\mathrm{thr}}\left(1 - \frac{c}{2}\right)\left(\frac{1}{2} + \frac{u}{2}\right), \\
r_A &= g^-\left[\frac{1}{4} + \frac{3}{4}\max(a_{\mathrm{other}}, b_{\mathrm{obs}})\right]\left(\frac{1}{2} + \frac{c}{2}\right), \\
r_S &= l\left(\frac{1}{2} + \frac{i_{\mathrm{rev}}}{2}\right), \\
r_H &= h_{\mathrm{self}}, \\
r_G &= v_{\mathrm{norm}} a_{\mathrm{self}}\left(\frac{1}{2} + \frac{h_{\mathrm{other}}}{2}\right).
\end{aligned}
\]

All appraisal fields take values in \([0,1]\); their meanings are as follows:

| Symbol | Meaning |
| --- | --- |
| \(g^+,g^-\) | Gain and harm appraisals; may incorporate sentiment bias when evidence is ambiguous |
| \(t_{\mathrm{thr}}\) | Threat level |
| \(c,u\) | Controllability and outcome uncertainty |
| \(a_{\mathrm{other}},a_{\mathrm{self}}\) | Attribution of responsibility to others and self |
| \(b_{\mathrm{obs}}\) | Degree of goal obstruction |
| \(l,i_{\mathrm{rev}}\) | Incurred loss and its irreversibility |
| \(h_{\mathrm{self}}\) | Overall negative self-evaluation |
| \(v_{\mathrm{norm}}\) | Degree of norm violation by the agent’s own behavior |
| \(h_{\mathrm{other}}\) | Degree of harm inflicted on others by the agent’s own behavior |

A temperament-gain function then converts \(r_i\) into a projected value. Temperament gain \(g_i\) is distinct from the gain/harm fields \(g^\pm\) and must satisfy \(0<g_{\min}\le g_i\le g_{\max}<\infty\):

\[
\phi_g(r) = \frac{g r}{1 + (g-1)r}, \qquad a_i = \phi_{g_i}(r_i).
\]

\(a\) is the encoded result of appraisal, not an emotional response that has already occurred. For sustained inputs and reassess, it is the reaction target; for stimulate, it controls a one-time relative increase. Only axes enabled by the mask are updated by that input; an unevaluated axis is not equivalent to an axis evaluated as zero. The full appraisal process follows the principle that complete evidence takes precedence, whereas existing tendencies may influence appraisal when evidence is ambiguous. The specific coefficients and functional forms are defined by this work.

### 4.3 Moving Restriction Box

The long-term background \(m\in[0,1]^{10}\) directly determines the allowable region of short-term mood. Restriction strength satisfies

\[
0<\ell_{\min}\le\ell_i\le\ell_{\max}\le0.999.
\]

For each axis:

\[
L_i(m) = \ell_i m_i, \qquad U_i(m) = \ell_i m_i + 1 - \ell_i,
\]

\[
\mathcal{C}(m) = \prod_{i=1}^{10} [L_i(m), U_i(m)].
\]

Short-term mood must satisfy \(x \in \mathcal{C}(m)\); its relation to the internal coordinate \(w\in[0,1]^{10}\) is:

\[
x_i = \ell_i m_i + (1 - \ell_i) w_i.
\]

Consequently, no matter how strong a single event’s appraisal may be, short-term mood \(x\) must lie within the upper and lower bounds set by the long-term background; proximal reaction \(e\) is not directly constrained by this box. When the background value of a negative-emotion axis is positive, the lower bound of the corresponding mood axis is also positive; when the background is low, the upper bound limits the instantaneous mood peak of that axis. The shape and strength of the restriction box are engineering choices and may attenuate the expression of strong proximal reactions in mood output.

### 4.4 Continuous Evolution and Event Jumps

The psychological state is \(X = (b, \{s_o\}, m, e, w, f)\), where \(b,m,e,w\in[0,1]^{10}\), \(s_o\in[0,1]^2\), \(f\in[0,1]\), and \(b\) is additionally restricted to its anchoring region. L1 also stores the amount awaiting integration \(Q_E\), the weighted sum across all axes \(P^E\), the cumulative integrated amount \(\Xi_E\), and the cumulative discarded amount \(D_E\). The last two are accounting quantities, not psychological time.

Principal continuous equations (simplified illustration):

- L1: Experience integration across all axes, with a capacity limit, discard rules, and a rate bound; \(\dot{b}_i\) evolves toward a convex target with an explicit conservation rule for the accounting quantities.
- L3: \(\dot{m}_i = \lambda_{m,i}[(1-\kappa)b_i + \kappa e_i - m_i]\), where \(\lambda_{m,i}>0\), \(0\le\kappa<1\).
- L4: Sustained drive + intentional regulation (weakened by fatigue) + attentional disengagement
- L5: Mood filtering + fatigue load/recovery equation

The proximal reaction of L4 and the mood filter of L5 are respectively:

\[
\dot e_i=\lambda_{e,i}(T_i-e_i)
-K_i[e_i-m_i]_+
-D_i^{\mathrm{att}}[e_i-T_i]_+,
\]

\[
\dot w_i=\lambda_{x,i}\left[
\operatorname{clip}_{[0,1]}\!\left(\frac{e_i-\ell_i m_i}{1-\ell_i}\right)-w_i
\right].
\]

where \(\lambda_{e,i},\lambda_{x,i}>0\), \([z]_+=\max(z,0)\). \(T_i\in[0,1]\) is the current drive target formed jointly by the long-term background and effective sustained sources; when no effective source exists, \(T_i=m_i\). The intentional down-regulation coefficient is \(K_i=\eta_i u_{\mathrm{reg},i}/(1+\rho_f f)\), where \(u_{\mathrm{reg},i}\in[0,1]\) is the actual regulatory effort limited by total activity capacity and \(\eta_i,\rho_f\ge0\). The attentional-disengagement coefficient is \(D_i^{\mathrm{att}}=\eta_i^{\mathrm{att}}d_i^{\mathrm{away}}\), where \(\eta_i^{\mathrm{att}}\ge0\), \(d_i^{\mathrm{away}}\in[0,1]\). The two down-regulation terms act on the portions of the reaction that exceed the background and the current drive target, respectively.

Short-term mood \(x\) is reconstructed from \(m,w\) according to Section 4.3; the fatigue equation of L5 appears in Section 4.6.

Events occur in two modes:

- stimulate: one-time relative increase; for each event \(r\) and axis \(i\), a shared flow length \(\vartheta_{ri}\) advances \(e_i\) and \(w_i\) along the same event flow. This flow length is not psychological time.
- reassess: bidirectional reappraisal toward absolute targets, updating toward the target by a finite dose without directly overwriting to the target value. When an entire axis reaction is to be lowered, the reappraisal scope must cover all effective causes of that axis; removal of a local cause updates only the corresponding source.

Batches at the same abstract instant are processed in semantic order, not in the order in which they reach the program.

### 4.5 Affective-Style Adaptation (L1)

L1 permits only slow, bounded adaptation of the affective baseline \(b\). The system maintains a capacity-limited experience summary \(Q_E\) and a weighted sum across all axes \(P^E\), satisfying

\[
0\le P_i^E\le Q_E\le Q_{\max},\qquad Q_{\max}>0.
\]

The experience summary includes all ten axes, including axes that are actually weak or absent; for eligible experience with a processing interval, the default summary is the average proximal reaction \(e\) over the period of actual participation, rather than a single peak. Eligible experience is integrated at a bounded rate; unintegrated portions may be discarded. Define

\[
a_E^c=\frac{\mu_Eu_E}{\delta_E+Q_E},\qquad
r_E=a_E^cQ_E\le\mu_E,
\]

where \(\mu_E,\delta_E,H_b>0\), \(u_E\in[0,1]\); \(a_E^c\) is the experience-consumption rate coefficient, \(r_E\) is the actual integration rate, and \(H_b\) is the experience-quantity scale. The baseline update equation is:

\[
\dot{b}_i = \frac{a_E^c}{H_b} \left[ (1-\rho_b) b_{0,i} Q_E + \rho_b P_i^E - Q_E b_i \right],
\]

where \(0\le\rho_b<1\), \(b_0\in[0,1]^{10}\). When the initial value satisfies

\[
b_i(0)\in\mathcal B_i,
\qquad
\mathcal B_i=[(1-\rho_b)b_{0,i},\;(1-\rho_b)b_{0,i}+\rho_b],
\]

and the summary remains within the admissible domain above, \(b_i\) remains inside \(\mathcal B_i\) for all time. When \(Q_E=0\) one also has \(P_i^E=0\), so the baseline update is identically zero and \(P_i^E/Q_E\) need not be computed. Personality adaptation does not obtain authority to modify memory content, retrieval rules, or replay strategies; replacement due to capacity limits and discarding affect only subsequent contributions to \(b\) and do not delete external memory.

### 4.6 Fatigue and Regulation

Subjective mental fatigue \(f \in [0,1]\) is driven by actual activity load and recovery opportunity:

\[
\dot{f} = a_f \mathcal{L} (1 - f) - b_f \mathcal{R}_f f.
\]

where \(a_f,b_f>0\), \(\mathcal L,\mathcal R_f\in[0,1]\); \(\mathcal L\) is the weighted load of actual activity and \(\mathcal R_f\) is the recovery opportunity. Recovery opportunity is given by \(\mathcal R_f=q_{\mathrm{rest}}(1-u_\Sigma)\), where \(q_{\mathrm{rest}}\in[0,1]\) is recovery quality and \(u_\Sigma\in[0,1]\) is the total effort currently in use. A low load weight does not imply that activity has ceased, nor does it automatically equal sufficient rest.

When actual regulatory effort and other conditions are held equal, fatigue does not increase the effective down-regulation coefficient; a default compensation strategy allows increased effort, yet under fixed current demand the effective down-regulation coefficient still does not increase with fatigue. This does not imply that peak emotional responses, cumulative emotional responses, or recovery times over a complete trajectory are monotonically related to fatigue.

Continuous evolution of proximal reactions comprises approach toward the current drive target, intentional down-regulation, and attentional disengagement. Fatigue does not directly reset emotional state, nor is it equivalent to insufficient motivation or task failure. It serves as the eleventh auxiliary output and is not the eleventh component of the personality-learning summary; nevertheless, through regulation it can alter \(e\) and thereby indirectly affect the ten-axis experience summary.

### 4.7 Sentiments and Ambiguous Appraisal

For each object, positive and negative tendencies \(s_o^+, s_o^- \in [0,1]\) are maintained separately. When event evidence is insufficient, existing object-directed tendencies may partially fill gaps in gain, harm, or threat appraisals; when evidence is complete they do not override explicit data. Learning of sentiments uses the direct evidence prior to addition of this bias, thereby avoiding self-reinforcing cycles. Pure memory replay does not automatically constitute new evidence for learning object-directed sentiments.

### 4.8 Output Quantization

The final output consists of eleven values. The first ten emotion axes employ quantization on a grid with spacing 0.001 that preserves the spatial restriction:

\[
K_i^{\min}=\lceil1000L_i(m)\rceil,\qquad
K_i^{\max}=\lfloor1000U_i(m)\rfloor,
\]

\[
y_i=\frac{1}{1000}\operatorname{clip}_{[K_i^{\min},K_i^{\max}]}
\bigl(\lfloor1000x_i+0.5\rfloor\bigr).
\]

Because \(1-\ell_{\max}\ge0.001\), every allowable interval contains at least one point on the 0.001 grid, so quantization is well-defined and guarantees \(y^{\mathrm{emo}}\in\mathcal C(m)\) together with \(|y_i-x_i|<0.001\). Here \(y^{\mathrm{emo}}\) denotes the first ten emotion axes.

The fatigue axis uses ordinary nearest-grid quantization:

\[
y_{\mathrm{Fat}}=\frac{\lfloor1000f+0.5\rfloor}{1000},
\qquad |y_{\mathrm{Fat}}-f|\le0.0005.
\]

The error bounds above are quantization bounds relative to the exact internal state; finite-precision implementations must additionally account for state-approximation, boundary-evaluation, and rounding errors.

Quantized values are not written back into the next step of the dynamics. Internal changes smaller than 0.001 may still accumulate.

## 5. Mathematical Properties and Guarantees

This section collects the principal properties satisfied by the foregoing equations under the conditions explicitly listed. These results are mathematical derivations and do not establish validity as a model of human emotion.

### 5.1 Existence, Uniqueness, and Invariant Domains

Inside each fixed mode, the number of objects and simultaneously active sources is finite, and exogenous inputs are bounded and measurable with respect to abstract time. If an input or appraisal is determined by internal state, its continuous part must be locally Lipschitz in the state and solved jointly with the state equations; local bounds and local Lipschitz constants of the composite vector field must be integrable on finite time intervals.

Further requirements are that initial values and parameters be admissible, that event maps be single-valued and domain-preserving, that event ordering be deterministic, that switches of events, modes, sources, and overrides be locally finite, and that undefined algebraic cycles and infinitely many switches in finite time (Zeno) be excluded. Under these conditions and a finite-time continuation condition, the system possesses a unique hybrid trajectory on any finite abstract-time interval, formed by joining the Carathéodory solutions within each mode through the event maps.

The psychological state \(b,m,e,w,s_o,f\) and the current L1 summaries \(Q_E,P^E\) remain inside their respective admissible domains. Cumulative accounting quantities \(\Xi_E,D_E\) remain finite on finite time intervals but are not required to possess a fixed upper bound over an unlimited period of use; a fixed-size summary does not imply that all deduplication and archival records remain fixed in size indefinitely.

### 5.2 Preservation of the Moving Restriction Box

From \(x_i = \ell_i m_i + (1-\ell_i) w_i\) and \(w_i \in [0,1]\), short-term mood always satisfies:

\[
x \in \mathcal{C}(m).
\]

During event updates \(m\) does not jump and the event flow likewise keeps \(w\in[0,1]^{10}\), so the restriction-box relation is preserved under both stimulation and reappraisal. Emotion-axis outputs, after the inward grid mapping described above, also lie inside \(\mathcal{C}(m)\).

### 5.3 Boundedness and Convergence of Personality Adaptation

With admissible initial values and summaries, the affective baseline remains inside \(\mathcal B_i\), hence

\[
|b_i-b_{0,i}|\le\rho_b,\qquad
|\dot b_i|\le\frac{\rho_b}{H_b}r_E\le\frac{\rho_b\mu_E}{H_b}.
\]

If the total quantity of integrated experience is finite, i.e., \(\int_0^\infty r_E(\tau)\,d\tau<\infty\), then \(b\) has finite total variation and converges to some \(b_\infty\). This limit need not equal the original temperament anchor \(b_0\). Neither discarding unintegrated summaries nor replacing them due to capacity limits directly changes the established baseline \(b\) or deletes external memory.

### 5.4 Other Conditional Properties

- When the baseline \(b=b_*\) is fixed and no new events or sustained emotion sources exist, if the dynamical rates satisfy the required conditions and regulation and attentional-disengagement coefficients are non-negative and bounded, then \(e,m,w,x\to b_*\).
- If after some instant no new external emotion events occur, non-replay sustained emotion sources cease, the sum of effective doses of replay events and the integral of effective source strengths of sustained replay are both finite, and \(b\) is fixed or has finite total variation, then \(e,m,w,x\to b_\infty\). Unresolved sustained concern does not satisfy the premise that sources have ceased.
- Emotion recovery does not automatically imply that fatigue returns to zero. If in addition \(\mathcal L\to0\) and ultimately \(\mathcal R_f\ge r_0>0\), the fatigue equation yields \(f\to0\).
- When appraisal, mask, and effective dose are fixed and \(m\) is held constant, pure-stimulate maps on \(e,w,x\) commute. This conclusion does not extend directly to the full system that includes sentiment learning and experience logging; mixed sequences of stimulate and reassess maps generally do not commute.
- Joint existence and uniqueness with the memory module is a conditional result: the vector fields of both modules and the continuous coupling maps must satisfy the corresponding time-measurability, state-local Lipschitz, and integrable-bound conditions; joint events must have deterministic ordering, single-valued maps, domain preservation, finite-time continuation, and a non-Zeno guarantee. Instantaneous snapshot-capture maps that include quantization may be discontinuous, so guarantees based on continuous Lipschitz feedback do not apply directly. Stored historical snapshots may instead be treated as part of the memory state; the applicable conditions must be assessed from the actual dependencies.

All guarantees above depend on input contracts and parameter domains. Arbitrary external closed loops, undefined event-generation rules, or validity as a model of human emotion lie outside the scope of this section.

## 6. Engineering Interfaces and Implementation Contracts

This section excerpts the principal contracts governing state persistence, event handling, numerical output, and cross-module collaboration.

### 6.1 State and Initialization

The core psychological state comprises \(b,\{s_o\},m,e,w,f\), together with L1 summaries and accounting quantities \(Q_E,P^E,\Xi_E,D_E\). At initialization every quantity must lie in its admissible domain, in particular \(b_i\in\mathcal B_i\) and \(0\le P_i^E\le Q_E\le Q_{\max}\). When only \(x,m\) are supplied, first verify \(x\in\mathcal C(m)\), then recover the in-box coordinate by \(w_i=(x_i-\ell_i m_i)/(1-\ell_i)\). Invalid values must be rejected or repaired with the correction explicitly documented; they must not be silently reset.

Complete saving and restoration also require retaining parameters, time and units, definition version, source and cause scopes, activity and effort overrides, event ordering, evidence deduplication, pending fragments, memory-quota allocation and consumption, accumulated integral summaries and consumed shares of ongoing experiences, and the life cycles of tasks and concerns. Saving only the core psychological state or the eleven-axis output is insufficient to restore the entire system.

### 6.2 Event Contract

Events are defined by changes in semantic facts, rather than by messages or tokens. Each event must have a distinct identifier, evidence set, reliability, relevance, and ordering information. Fragments of the same canonical event must first be merged, after which saturation is applied; retransmission does not generate an additional dose.

Memory replay uses a dose constrained by a quota; aliases and versions of the same historical evidence share a quota ledger, and both one-shot and sustained recall consume the same quota. For pure replay, \(q^{\mathrm{learn}}=0\) and no new L1 experience share is allocated. Reflection that adds new verifiable conclusions must be assessed separately for experience eligibility under a distinct identifier; actual recall processing may still integrate previously eligible summaries, but this does not constitute new evidence.

### 6.3 Output Quantization

Mood axes employ inward grid quantization under the restriction-box width condition stated earlier; fatigue uses ordinary nearest-grid quantization. Finite-precision implementations must guarantee that evaluation of interval endpoints and integer grid points conforms to the quantization definition; total error relative to the exact trajectory must also include numerical approximation error of the internal state.

Quantized values are not written back into the next step of the dynamics. The eleven-axis output is not a complete state from which the system can be restored; saving and restoration must include internal state together with its inputs, deduplication, and quota accounting.

### 6.4 Boundary with the Memory Module

- The emotion module may emit time-stamped emotion snapshots for the memory module to store according to its own rules.
- Personality learning must not modify memory content, weights, retrieval rules, replay selection, or reconsolidation strategies.
- When the joint interface is enabled, modules establish correspondence via the same real elapsed time \(t\) but may possess independent abstract scales; this common time contract does not imply that a real clock has been connected at the present stage.
- Historical snapshots retain the quantized values and definition version captured at the time of recording and must not be clipped again using the current state.

### 6.5 Initial Numerical Settings for Implementation

The model specifies a set of initial numerical settings for implementation (including layer rates, restriction strengths, experience capacity, fatigue parameters, etc.). These values are merely a starting point for a working implementation, have not been calibrated against human data, and must be used while preserving all parameter-domain and rate-separation conditions.

## 7. Scope, Limitations, and Outstanding Work

- The model deliberately does not construct subsystems for physiology, hormones, sleep, or circadian rhythms.
- Personality evolution is limited to affective style and emotion-expression-related tendencies and is not authorized to rewrite other modules.
- The ten axes constitute a finite representation; surprise, gratitude, love, etc., are not independently output, nor is cross-cultural equivalence claimed.
- The model has not yet been fully integrated with a memory theory; joint guarantees are conditional and require subsequent memory specifications to satisfy the listed interfaces.
- The initial settings for implementation are uncalibrated; no validation against human emotional responses has yet been conducted.
- The eleven-axis output discards object-directed sentiments, internal-state decomposition, and precision finer than 0.001, and cannot be used alone for complete restoration.

## 8. Conclusion

This paper proposes a five-layer emotion dynamics model (HFEDM) oriented toward dialogue and task contexts. By means of computable state equations, the moving restriction box \(\mathcal{C}(m)\), bounded affective-style adaptation, and ownership boundaries with the memory module, the model aims to provide a consistent and bounded representation of emotional state under limited resources.

The present work remains a hypothetical theory of emotion dynamics and mathematical specification; its deliverables comprise formal definitions, initial settings for implementation, and interface contracts. Future work may focus on integration with a complete memory theory, parameter calibration, and system implementation. The paper does not claim to have solved the problem of modeling human emotion, nor does it assert that the current equations have received empirical support.

## References

Beedie, C. J., Terry, P. C., & Lane, A. M. (2005). Distinctions between emotion and mood. *Cognition and Emotion, 19*(6), 847–878. [https://doi.org/10.1080/02699930541000057](https://doi.org/10.1080/02699930541000057)

Gebhard, P. (2005). ALMA: A layered model of affect. In *Proceedings of the Fourth International Joint Conference on Autonomous Agents and Multiagent Systems* (pp. 29–36). ACM Press. [Publication record at the author’s institution](https://www.dfki.de/en/web/research/projects-and-publications/publication/1782/)

Grillon, C., Quispe-Escudero, D., Mathur, A., & Ernst, M. (2015). Mental fatigue impairs emotion regulation. *Emotion, 15*(3), 383–389. [https://doi.org/10.1037/emo0000058](https://doi.org/10.1037/emo0000058)

Hofmann, W., De Houwer, J., Perugini, M., Baeyens, F., & Crombez, G. (2010). Evaluative conditioning in humans: A meta-analysis. *Psychological Bulletin, 136*(3), 390–421. [https://doi.org/10.1037/a0018916](https://doi.org/10.1037/a0018916)

Smith, C. A., & Ellsworth, P. C. (1985). Patterns of cognitive appraisal in emotion. *Journal of Personality and Social Psychology, 48*(4), 813–838. [https://doi.org/10.1037/0022-3514.48.4.813](https://doi.org/10.1037/0022-3514.48.4.813)

Wrzus, C., & Roberts, B. W. (2017). Processes of personality development in adulthood: The TESSERA framework. *Personality and Social Psychology Review, 21*(3), 253–277. [https://doi.org/10.1177/1088868316652279](https://doi.org/10.1177/1088868316652279)
