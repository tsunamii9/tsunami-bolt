**THESIS REVIEW – CORRECTIONS TO BE MADE BY THE RESEARCH SCHOLAR**

*Based on the review of the submitted revised thesis*

**Overall Recommendation:** Major corrections are required before the thesis is considered ready for final submission/examination. The research work is substantial and technically promising, but methodological clarity, validation, terminology, language, formatting, and the strength of several claims need improvement.

**Important:** The corrections below are intended to improve the academic defensibility, reproducibility, consistency, and presentation of the thesis. The research scholar should revise the thesis carefully and verify every change against the actual implementation, experiments, data, and references.

**1\. Research Contribution and Novelty**

-   Clearly state the single most important original contribution of the thesis.
-   For each proposed component—BC-IDMF, BC-DWSR, KBGA-GRU, KBGA-RF/QoES framework, and the integrated BC-SWSC architecture—clearly distinguish the proposed novelty from the underlying established techniques.
-   For every proposed method, explain: existing approach → identified limitation → proposed modification → novelty → experimental evidence of improvement.
-   Clarify whether the principal contribution is algorithmic, architectural, methodological, or a combination. If the main contribution is architectural/integrative, state this consistently throughout the thesis.
-   Ensure that claims of originality are supported by the literature review and comparison with closely related recent studies.

**2\. Research Gap, Objectives and Research Questions**

-   Strengthen the final research-gap statement so that it clearly explains what existing SWSC research does not adequately address.
-   Ensure a direct one-to-one mapping among Research Gap → Research Objectives → Research Questions → Proposed Methods → Experiments → Results → Conclusions.
-   At the end of Chapter 1, provide a consolidated table showing this complete mapping.
-   Ensure that each research question is explicitly answered in the final chapter using the corresponding experimental evidence and quantitative results.

**3\. Dataset and Experimental Data – IMPORTANT**

-   Clearly identify the source of the dataset containing 1,083 web services.
-   State whether the data are real, publicly available, synthetic, constructed, enriched, or a combination.
-   Provide the original dataset size, preprocessing steps, duplicate removal, missing-value handling, enrichment procedure, final number of records, attributes/features, and class distribution.
-   Clearly describe the method used to generate or assign ground-truth labels, if applicable.
-   State the exact training/testing split and explain whether the split was random, stratified, or otherwise controlled.
-   If synthetic or illustrative personal information appears in tables, explicitly label it as synthetic/anonymized experimental data. Do not present fabricated identities as real users.
-   Add a subsection titled 'Data Privacy and Ethical Considerations' where appropriate.
-   Provide sufficient dataset and preprocessing information to allow another researcher to reproduce the experiments.

**4\. MAUT-Based Evaluation of BC-IDMF – IMPORTANT**

-   Provide a clear mathematical and methodological explanation of the MAUT calculation used to obtain the reported legitimacy scores.
-   Explain the origin and justification of every attribute score assigned to SAML, OpenID, SSI-IDM and BC-IDMF.
-   Explain how attribute weights were selected and whether all attributes have equal weight.
-   If expert judgment was used, report the number/profile of experts, scoring procedure, aggregation method, and inter-rater agreement where applicable.
-   Add a sensitivity analysis showing whether the ranking changes under reasonable alternative weights.
-   Use precise wording such as 'under the defined MAUT scoring scheme' rather than presenting the resulting percentage as an independently measured universal security score.
-   Check the number of attributes and the stated maximum total score for mathematical consistency.

**5\. Threat Model and Security Claims**

-   Define the attacker/threat model clearly before presenting security experiments.
-   For each attack scenario, specify attacker capability, attack objective, assumptions, attack procedure, expected impact, and proposed mitigation.
-   Substantiate claims relating to 51% attacks, selfish mining, fraudulent miners, threshold manipulation, permission tampering, and fake credential injection.
-   Avoid stating that blockchain itself guarantees security, privacy, or truthfulness. Distinguish ledger immutability from the correctness of off-chain data, credentials, keys, or oracles.
-   Where the proposed mechanism only reduces risk under specified assumptions, use wording such as 'mitigates' or 'reduces the risk' rather than 'prevents' or 'guarantees'.
-   Add a concise threat-model table mapping each threat to the proposed mitigation and experimental evidence.

**6\. Miner Selection and Performance Experiment**

-   The experiment using 5, 10, 15, 20, 25, 30, 40, 60, 80 and 100 miners should include adequate statistical analysis.
-   Report mean, standard deviation and, where appropriate, confidence intervals for repeated measurements.
-   Explain the choice of five independent runs and discuss its adequacy.
-   Where multiple groups are compared, use an appropriate statistical test and report the test statistic, p-value, and effect size where applicable.
-   Define mathematically what is meant by an 'optimal' miner count. If the conclusion is based on a performance-versus-cost trade-off, explicitly define that trade-off.
-   Avoid describing 20 miners as universally optimal unless the evidence supports such a generalization.

**7\. KBGA-GRU and Machine Learning Evaluation**

-   Clearly describe the experimental protocol, including dataset split, preprocessing, hyperparameters, random seeds, number of independent runs, and hardware/software environment.
-   Report results as mean ± standard deviation when repeated experiments are performed.
-   Report exact p-values for statistical hypothesis tests rather than only stating that results are significant.
-   Where applicable, report effect sizes and confidence intervals.
-   Explain whether the reported improvements are absolute percentage-point improvements or relative percentage improvements.
-   Ensure that all comparison models are evaluated under identical conditions.
-   Clarify how hyperparameters were selected for every baseline model to avoid unfair comparison.
-   Avoid the word 'conclusively' when the evidence is limited to the evaluated dataset and experimental conditions. Prefer wording such as 'the results indicate' or 'the proposed model outperformed the evaluated baselines under the experimental conditions.'
-   Check for data leakage between training and testing sets, particularly where feature selection or optimization is involved.

**8\. QoS/QoE and Service Selection**

-   Standardize terminology throughout the thesis. Use QoS (Quality of Service) and QoE (Quality of Experience) consistently where these are the intended concepts.
-   Clearly explain how the authenticity/reliability of QoE feedback is established.
-   Explain what blockchain guarantees about the feedback and what it does not guarantee. Immutable storage does not by itself prove that the original feedback was truthful.
-   Clearly document the KBGA-RF feature-selection process, including input features, selection criteria, number of selected features, and comparison with non-optimized feature selection.
-   Provide sufficient details to reproduce the Fuzzy TOPSIS service-ranking procedure, including membership functions, weights, normalization and decision criteria.

**9\. End-to-End Case Study**

-   Clearly describe the experimental environment used for the e-commerce/mobile purchasing case study.
-   State whether the case study is a simulation, prototype, emulation, or actual deployment.
-   Avoid calling the case study 'real-world' unless an actual operational environment was used.
-   Explain how the centralized baseline and proposed blockchain-based system were configured under equivalent conditions.
-   Provide sufficient information to reproduce the attack scenarios and service-composition experiments.
-   Clearly distinguish scenario-based validation from large-scale real-world deployment.

**10\. Blockchain Platform and Reproducibility**

-   The thesis refers to the Ropsten testnet. Explain why it was selected, when the experiments were conducted, and how the experiments can be reproduced now that Ropsten is deprecated.
-   Provide blockchain platform/version, smart-contract language/version, libraries, compiler version, node/client configuration, and relevant network parameters.
-   Provide the experimental hardware and software environment, including CPU, RAM, operating system, programming language, major libraries/frameworks and versions.
-   Where possible, provide source code, configuration files, algorithms/pseudocode, datasets or synthetic-data generation procedures as supplementary/reproducibility material.

**11\. Literature Review**

-   Strengthen the critical analysis in Chapter 2. Reduce purely descriptive background and emphasize comparison, limitations, contradictions and unresolved problems.
-   For each major research area, use the structure: existing approaches → limitations → research gap → relevance to the present thesis.
-   Add a consolidated research-gap matrix mapping important previous studies to their limitations and the corresponding contribution of this thesis.
-   Verify that the literature review contains sufficient recent work, particularly from 2022–2026, on blockchain identity, decentralized identity, SWSC, ZKP, smart-contract security, blockchain-based service discovery and AI-assisted service composition.
-   Verify every citation against the actual reference list.

**12\. Claims and Academic Language**

-   Moderate claims that are stronger than the experimental evidence.
-   Prefer 'tamper-evident' or 'tamper-resistant' to 'tamper-proof' where technically appropriate.
-   Prefer 'supports trust' or 'enhances trust under the stated assumptions' to 'guarantees trust'.
-   Replace 'conclusively proves' with evidence-based wording unless a formal proof is actually provided.
-   Clearly distinguish theoretical analysis, simulation, prototype evaluation and real-world deployment.
-   Avoid implying that blockchain alone establishes the truthfulness of off-chain information.

**13\. Terminology and Consistency**

-   Correct and standardize technical terminology throughout the thesis.
-   Use 'Gated Recurrent Unit (GRU)' consistently.
-   Check the use of 'Capacity & Satisfaction Evaluation Framework' and ensure it is consistent with the intended QoS/QoE terminology.
-   Review abbreviations such as SSC and remove or define any abbreviation that is not subsequently used meaningfully.
-   Prepare an updated list of abbreviations and ensure that every abbreviation is defined at first use.

**14\. Formatting, Tables, Figures and Equations**

-   Correct the typographical error 'RESEARCH METHODDOLOGY' to 'RESEARCH METHODOLOGY'.
-   Check all chapter, section and subsection numbering against the Table of Contents.
-   Verify preliminary-page Roman numbering and main-text Arabic numbering.
-   Ensure every figure and table has a consistent caption format and is referred to in the text before/near its appearance.
-   Check all equations for correct symbols, numbering, alignment and cross-references.
-   Check for OCR/typing artifacts, broken symbols, unusual characters and formatting errors in mathematical notation.
-   Ensure tables fit within page margins and use consistent fonts, spacing and alignment.
-   Check all headings, subheadings, page breaks and blank spaces.
-   Ensure the final PDF is visually consistent page-by-page.

**15\. References and Citation Audit**

-   Verify that every in-text citation appears in the reference list.
-   Verify that every reference listed is cited in the thesis where appropriate.
-   Check author names, article titles, journal/conference names, volume, issue, pages, year and DOI.
-   Verify DOI links and remove incorrect or incomplete bibliographic information.
-   Use one consistent reference style throughout the thesis.
-   Check recent references and ensure that key claims are supported by appropriate primary research rather than only review papers.

**16\. Chapter 6 – Conclusions and Future Work**

-   Answer RQ1–RQ5 explicitly in the conclusion chapter.
-   For each research question, state the principal finding and at least one relevant quantitative result.
-   Map each research objective to its achieved result.
-   Separate demonstrated findings from proposed future work.
-   Retain and strengthen the limitations discussion, particularly regarding constructed/enriched data, scalability, consensus latency, storage/gas overhead, energy, governance, validator collusion, key management, platform portability and case-study scope.
-   Clearly state that the current evidence represents prototype-level/case-specific validation where applicable.

**17\. English Language and Academic Style**

-   Conduct a complete academic English proofreading of the thesis.
-   Correct grammar, sentence structure, punctuation, articles, subject-verb agreement and technical terminology.
-   Replace unnecessarily complicated or conversational sentences with concise academic language.
-   Avoid repetition between the Introduction, Literature Review, Methodology, Results and Conclusion chapters.
-   Check consistency of tense, especially in descriptions of methodology and completed experiments.
-   Have the final thesis proofread by a competent academic English editor before submission.

**18\. Final Quality-Control Checklist**

-   ☐ All research objectives are addressed.
-   ☐ All research questions are explicitly answered.
-   ☐ Research gap and novelty are clearly established.
-   ☐ Dataset source and construction are transparent.
-   ☐ Experimental methodology is reproducible.
-   ☐ Statistical tests are properly reported.
-   ☐ All tables and figures are verified.
-   ☐ All equations are verified.
-   ☐ All citations and references are cross-checked.
-   ☐ All security claims are consistent with the threat model.
-   ☐ MAUT methodology and scoring are justified.
-   ☐ ML comparisons are fair and statistically supported.
-   ☐ Synthetic/anonymized data are clearly identified.
-   ☐ Blockchain platform/version information is provided.
-   ☐ Ropsten limitation is explicitly addressed.
-   ☐ Terminology and abbreviations are standardized.
-   ☐ Language and grammar are professionally edited.
-   ☐ Formatting and page numbering are checked.
-   ☐ Final PDF has been proofread page-by-page.

**Overall Reviewer's Recommendation**

The thesis demonstrates substantial technical work and a coherent integrated research framework. **However, major corrections are recommended before final submission.** The revision should focus primarily on strengthening methodological transparency, statistical validation, dataset documentation, novelty justification, threat modelling, reproducibility, technical terminology, academic language and formatting. The scholar should not merely add text; existing claims, tables, experiments and conclusions should be carefully rechecked against the actual implementation and evidence.

**Suggested status:** MAJOR CORRECTIONS REQUIRED BEFORE FINAL SUBMISSION