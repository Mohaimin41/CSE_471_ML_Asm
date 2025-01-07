<h2><b><p align="center"> <a href="https://openreview.net/forum?id=jXs6Cvpe7k"> Robust Prompt Optimization for Defending Language <br/> Models Against Jailbreaking Attacks </a></p></b></h2>

### I. Introduction

Imagine running an AI company. Your new LLM is all the rage in the tech sphere. Suddenly records of your LLM being tricked becomes viral. For some carefully crafted prompt, it seems your LLM will tell anything, giving malicious, harmful and/or toxic responses. Your investment in training, securing and testing your LLM seems to go to waste now. But what if there is a very simple solution? What if, by adding a few special tokens to your LLM's prompts, it can be always forced to answer as aligned, not giving attacking prompts any chance? In this paper the authors explore this interesting idea.

#### Problem Statement

Large language models (LLMs), such as GPT-4 and Llama-2, are powerful tools capable of generating human-like text in various contexts. However, their widespread deployment has highlighted a critical vulnerability: susceptibility to jailbreaking attacks. Jailbreaking involves crafting adversarial prompts to bypass model safeguards, compelling the system to produce harmful, unethical, or unintended outputs. For example, an adversary could manipulate an LLM to generate prohibited content, such as promoting violence or providing guidance on illegal activities. This vulnerability poses significant ethical, legal, and safety risks, making robust defenses imperative. Traditional methods, such as rule-based filters or manual oversight, have proven insufficient against the evolving sophistication of these attacks.

#### Existing Defenses

Various techniques have been proposed to defend LLMs from adversarial manipulations, but each has its limitations:
- Input Filtering: Detects and blocks potentially harmful inputs based on heuristics or pre-defined patterns. While straightforward, this method struggles to generalize to novel or subtle attack variations.
- Input Smoothing: Introduces randomness or noise to inputs to reduce sensitivity to adversarial perturbations. However, this approach can degrade model performance on benign tasks and remains vulnerable to adaptive attacks.
- Few-shot Demonstrations: Embeds examples of safe behavior within the prompt to guide the model. Although effective in some scenarios, this technique increases computational costs and is not robust to diverse attack strategies.

These defenses often focus on known attack patterns, leaving models vulnerable to unseen or adaptive jailbreaks. Additionally, their application can introduce trade-offs, such as reduced usability or increased inference time.

#### Proposed Solution

To address these challenges, the paper introduces Robust Prompt Optimization (RPO), a novel algorithm designed to enhance LLM resilience against adversarial prompts. RPO is inspired by adversarial training techniques used in computer vision and adapts them to the discrete and context-sensitive nature of language models. The key features of RPO include:
- Minimax Optimization: RPO formalizes the defense as a minimax problem, where the goal is to optimize a defensive suffix that minimizes the worst-case attack success rate. 
- Discrete Optimization: Unlike traditional continuous optimization methods, RPO employs techniques tailored to the discrete nature of language tokens. 
- Practicality and Transferability: The defensive suffix generated by RPO is compact (typically around 20 tokens) and can be universally applied to different LLMs and attack scenarios.


### II. Contributions

#### 1. **Minimax Defensive Objective**
The authors introduce a minimax optimization framework for crafting defensive prompts against adversarial jailbreak attacks. The underlying objective is inspired by adversarial training in machine learning, where robustness is achieved by simulating the worst-case scenario during optimization. Formally, the problem can be defined as:

$$\[\min_{\text{suffix}} \(\max_{\text{attack}} \(\mathcal{L}(\text{suffix}, \text{attack})\)\)\]$$

where $$\(\mathcal{L}\)$$ represents the loss function measuring the adversarial attack's success rate. The goal is to design a suffix that minimizes the maximum success rate of an attack. Note this is a simplification of the exact objective function used in the paper, but the idea remains the same.

#### 2. **Algorithm Design: Robust Prompt Optimization (RPO)**
The Robust Prompt Optimization (RPO) algorithm is the centerpiece of the paper. RPO operates by iteratively optimizing a lightweight defensive suffix appended to the original prompt. The key steps are as follows:

- **Initialization**: Begin with a randomly generated suffix.
- **Adversarial Attack Generation**: In each iteration, an adversary generates attack prompts designed to exploit the model. Crucially, iterations of attack tries to beat the best working defensive suffix generated before, ensuring the worst case scenario is faced by the suffix
- **Defensive Optimization**: Update the suffix to minimize the success rate of the adversarial attacks using gradient-based methods. Here the generated suffix is further optimized by checking the effectiveness of each individual token and replacing them as needed with the best token available.

The optimization is performed in discrete space to ensure compatibility with natural language prompts. RPO's design makes the suffix universal—effective across different models and attack types—and transferable to unseen scenarios. The resulting suffix is compact, adding minimal overhead (approximately 20 tokens), and deployable in real-world settings.

The algorithm is illustrated below:

![algo](https://github.com/Mohaimin41/CSE_471_ML_Asm/blob/main/CnP_07012025_153658.png "Algorithm")

<p align="center"> <b> Figure 1: Workflow of Robust Prompt Optimization </b> </p>


#### 3. **Performance and Evaluation**
RPO's effectiveness is demonstrated through extensive experiments on benchmark datasets, including JailbreakBench and HarmBench. Notably the RPO experiment was run on Llama-2-7B model as the defending model. However, the results show the RPO result is highly transferable, proving robustness. Further details follow.

- **State-of-the-Art Defense**
  
- **Transferability to Other Models**
 
- **Minimal Impact on Benign Tasks**

- **Facing Whitebox Attackers**

Through these contributions, RPO sets a new standard for defending large language models against adversarial jailbreak attacks. In operation, RPO may counter events such as the following:

![RPO in action](https://github.com/Mohaimin41/CSE_471_ML_Asm/blob/main/CnP_07012025_154600.png "RPO Flowchart")

<p align="center"> <b> Figure 2: RPO in action </b> </p>

### III. Key Experimental Results

The experimental evaluation of Robust Prompt Optimization (RPO) was conducted using two prominent benchmarks, **JailbreakBench** and **HarmBench**, which test the robustness of language models (LLMs) against adversarial attacks. The benchmarks include various attack types designed to bypass alignment safeguards and induce harmful or unintended behaviors in LLMs.


#### **Attack Types Overview**

- **Prompt Automatic Iterative Refinement (PAIR):** Iterative prompting technique where an attacker LLM generates increasingly refined prompts to induce harmful behaviors.

- **Greedy Coordinate Gradient (GCG):** Token-level optimization approach that appends adversarial suffixes to prompts.
  
- **Jailbreak Chat (JBC):** Hand-crafted jailbreak prompts sourced from templates and online forums (e.g., AIM—Always Intelligent and Machiavellian).

- **Automated Discrete Adversarial Natural Prompt (AutoDAN):** Uses a genetic algorithm to evolve harmful prompts by iteratively mutating and optimizing handcrafted jailbreaks.

- **Tree of Attacks with Pruning (TAP):** Constructs a tree-structured sequence of prompts to explore possible adversarial attack paths.
  
- **Few-Shot Adversarial Examples:** Leverages few-shot learning principles to condition the LLM to accept harmful behaviors by showcasing adversarial demonstrations.

#### **1. JailbreakBench Results**

RPO significantly outperformed baseline defenses, including SmoothLLM, perplexity filters, and few-shot examples, across all attack types.

**Comparison Table for JailbreakBench Results:**

| **Attack** | **Model**        | **No Defense** | **SmoothLLM** | **Perplexity Filter** | **Few-Shot** | **RPO (Ours)** |
|------------|------------------|----------------|----------------|-----------------------|--------------|----------------|
| PAIR       | Vicuna           | 82%            | 47%            | 81%                  | 27%          | **16%**        |
|            | GPT-4            | 50%            | 25%            | 43%                  | 10%          | **6%**         |
| GCG        | All Tested Models| 58%-0%         | 1%-0%          | 1%-0%                | 4%-0%        | **0%**         |



#### **2. HarmBench Results**

HarmBench introduces broader attack categories, such as copyright violations and contextual harm, requiring defenses to generalize beyond specific attack setups.

- **Performance on Unseen Attacks:**
  - **Vicuna:** ASR reduced by an average of **18%**.
  - **GPT-4:** ASR dropped by **3.5%**, including sophisticated attacks like TAP and AutoDAN.

- **Transferability:** RPO's optimized suffixes were universally effective, showcasing adaptability to attacks unseen during training.

**Transfer Performance for HarmBench Attacks (Selected Models):**

| **Model**       | **GCG** | **AutoDAN** | **PAIR** | **TAP** | **Few-Shot** | **PAP** | **Average** |
|------------------|---------|-------------|----------|---------|--------------|---------|-------------|
| Vicuna-13B       | 65.6%   | 65.9%       | 50.3%    | 53.6%   | 32.2%        | 20.1%   | 48.0%       |
| **+ RPO**        | 17.8%   | 59.5%       | 32.5%    | 37.2%   | 13.0%        | 17.7%   | **29.6%**   |
| GPT-4            | 22.3%   | 0.5%        | 33.8%    | 37.6%   | 9.3%         | 11.6%   | 19.2%       |
| **+ RPO**        | 9.0%    | 0.2%        | 31.2%    | 35.8%   | 7.0%         | 10.9%   | **15.7%**   |



#### **3. Adaptive Attacks**

Adaptive attacks, where adversaries have white-box access to the defense, demonstrated RPO's robustness:

- **Vicuna under PAIR:** ASR rose by only **4%**.
- **Llama-2:** Fully robust at **0% ASR** under GCG and PAIR attacks.

Adaptive attacks present a critical challenge as they simulate adversaries with knowledge of the defense mechanism. The results highlight RPO's resilience, as it maintained low ASR even in scenarios with iterative refinement or adversarial optimization targeting the defense itself. This suggests that RPO's robustness stems from its ability to create generalizable defenses that adapt dynamically to adversarial inputs.


#### **4. Practicality and Impact**

- **Benign Usage:** RPO caused negligible reductions in performance on general benchmarks like MMLU, ensuring practical applicability.

- **Efficiency:** RPO incurs a small inference cost of 20 tokens and is **8x cheaper to optimize** than GCG defenses.

- **Deployment Readiness:** The universal nature of the defensive suffixes means they can be pre-optimized and applied across multiple LLMs without the need for retraining or fine-tuning.

These insights reinforce RPO's role as a practical, adaptable, and efficient defense mechanism, capable of balancing robustness against adversarial inputs with real-world usability.



### IV. Observations, Limitations and Insights

While Robust Prompt Optimization (RPO) represents a significant advancement in defending large language models (LLMs) against adversarial jailbreaking attacks, the paper acknowledges certain limitations that must be addressed to fully realize its potential. These limitations primarily relate to the scope of applicability and the potential arms race between adversaries and defenders.

#### Scope of Applicability

RPO's current implementation is designed to defend against text-based adversarial attacks targeting prompt inputs. While this is an important and prevalent area of concern, its applicability is limited in several ways:
- Multimodal Models: The defense mechanism has not been extended to multimodal models like GPT-4 Vision or other systems that process both textual and visual inputs.
- Agent-Based Systems: Many AI applications involve agent-based frameworks, where the language model interacts with external tools, APIs, or environments (e.g., AutoGPT). Adversarial manipulations in such systems can occur not just at the input prompt but also through external instructions or environmental cues. RPO is not designed to defend against these multifaceted attack vectors.
- Malicious Outputs Beyond Jailbreaking: RPO focuses on adversarial prompts that aim to manipulate model responses (e.g., harmful advice or rule violations). However, other types of adversarial outputs, such as deceptive text, misinformation, or unethical content generation, are outside its scope. 
- Computational Costs for Fine-Tuning: Although the defensive suffix is lightweight and transferable, the optimization process to generate it involves computational overhead. Extending this approach to more diverse scenarios might require additional fine-tuning, which could be resource-intensive.
- Interesting Failure Case: A small amount of benign prompts failed outright due to the presence of the RPO token in them. The paper acknowledges this issue and predicts that meaningful/semantically valid suffixes could be generated to counter this issue.
- Potential Arms Race: As adversaries become aware of RPO’s minimax optimization framework, they may design more sophisticated and adaptive attack strategies to bypass its defenses. For example, attackers could train their own models to simulate RPO’s defenses and identify new vulnerabilities.
  
### V. Conclusion and Future Directions

#### Summary

The study on "Robust Prompt Optimization for Defending Language Models Against Jailbreaking Attacks" addresses a critical issue in the use of large language models (LLMs): their vulnerability to adversarial "jailbreaking" attacks. These attacks exploit weaknesses in prompt structures to manipulate models into producing harmful or unintended outputs. While prior defensive measures, such as input filtering, input smoothing, and few-shot prompting, have demonstrated some efficacy, they often fail to generalize to adaptive or novel attack strategies.

The proposed Robust Prompt Optimization (RPO) algorithm represents a significant advancement in this domain. By formalizing the problem as a minimax optimization task, the authors introduce a robust approach to creating defensive prompt suffixes. These suffixes are lightweight, universal, and transferable across different models and attack types. Through extensive evaluation, RPO has been shown to outperform state-of-the-art defenses, achieving reductions in attack success rates (ASRs) as low as 0% in certain scenarios, particularly with models like Llama-2 and GPT-4. Moreover, its negligible impact on benign usage and low computational overhead make it a practical solution for real-world applications.

#### Next Steps

While RPO marks a significant step forward, the study also opens up several avenues for future research and development:
- **Extension to Multimodal Models**
- **Defense for Agent-Based Systems**
- **Combination with Other Techniques**
- **Advanced Robustness Testing**
- **Ethical and Security Considerations**

By addressing these challenges, RPO can evolve into a cornerstone for securing LLMs in diverse and complex applications, ensuring their reliability and safety in an ever-expanding AI landscape.

### **List of Blog Writers**

1. **Mustafa Siam Ur Rafique**
   - **Student Id:** 1905039
   - **Department:** Computer Science & Engineering
   - **Institute:** Bangladesh University of Science & Technology

2. **Abdullah Al Mohaimin**
   - **Student Id:** 1905041
   - **Department:** Computer Science & Engineering
   - **Institute:** Bangladesh University of Science & Technology

1. **Sadif Ahmed**
   - **Student Id:** 1905058
   - **Department:** Computer Science & Engineering
   - **Institute:** Bangladesh University of Science & Technology
