Karar Hayder's entry into the zaincash agentic AI competition 9/22/2026
Below is my full roadmap and thought process detailed

# Stage 0: Local vs API

This is a matter of cost, reliability, and maintainability.
We need low cost, high reliability, and high maintainability, while remaining scalable for increasing numbers of customers.

## 0.1 Self-Hosted LLM Server

According to industry sources, local AI GPU hardware for 70B+ models typically costs $4,200 for a professional RTX A6000 or $10,000+ for an A100, with a passive electricity cost of roughly $30–80+ per month, before accounting for physical maintenance fees or scalability limits.

Pros: Completely private environment for data privacy, fully tailored for specific needs.

Cons: Needs a fully trained team to maintain and operate, infrastructure is limited, and scalability can put service integrity at risk. Also needs backup infrastructure in case the main server fails, which doubles cost.

### Conclusion: Not preferable.

## 0.2 Cloud-Based LLM

This provides scalable infrastructure with the possibility of backups. The main provider considered was Amazon, as it offers multiple services, including several tailored specifically to AI.

Three main Amazon services to note:

### 0.2.1 Bedrock API
- Multiple AI models readily available
- Zero infrastructure management
- Highly controllable LLMs through Guardrails and knowledge bases

### 0.2.2 SageMaker Endpoints
- Can use custom fine-tuned LLMs
- More infrastructure management, better control
- Predictable token usage

### 0.2.3 Self-Hosted EC2
- Handles high token volume (>10B monthly)
- Requires a machine learning team to manage infrastructure
- Supports custom inference optimizations (vLLM, TensorRT-LLM, custom kernels)
- Can use 1–3 year reserved instances for the deepest discounts

### Conclusion

This option has high maintainability and reliability (backup LLM models when one fails), with cost varying by service.

| Option | Cost | Best at |
|---|---|---|
| Bedrock API | $0.22–0.44 per 1M tokens (7B model) | <10M tokens/month |
| SageMaker (ml.g5.xlarge) | ~$1,015/month | 10–50M tokens/month |
| Self-hosted EC2 (g5.xlarge) | $724/mo on-demand, $290/mo reserved | >50M tokens/month |

For hackathon purposes this is not preferable, but could be used at a larger scale depending on company needs.

Source: wring.co/blog/aws-llm-hosting-vs-api-costs, 2026.

## 0.3 Direct API LLM

This method uses direct API calls to AI providers with no middleman. It is easily maintainable, with reliability and cost varying by provider.

### 0.3.1 Anthropic's Claude

- Claude's main strength is logic — tasks such as coding or structured reasoning.
- Pricing varies by tier; flagship-tier pricing is roughly $5/$25 per million input/output tokens, with cheaper tiers available.
- Claude was prompted to create a scenario and respond in Iraqi Arabic:
"وكفة شوي — هاي أول مرة ترسل فلوس لهذا الحساب، والمبلغ أكبر من المعتاد بكثير. إذا وصلتك رسالة تگول فزت بجائزة وتحتاج ترسل فلوس حتى تستلمها، هاي طريقة نصب معروفة. الشركات الحقيقية ما تطلب منك ترسل فلوس عشان تستلم جائزة. تريد تكمل العملية؟"

### 0.3.2 Google's Gemini

- Cheapest per-token option among the three.
- Strongest at assistance-style tasks rather than logic-heavy work such as coding.
- Gemini was prompted to create a scenario and respond in Iraqi Arabic:
"عمي، دير بالك. هاي أول مرة تحول فلوس لهذا الرقم، والقصة تشبه سوالف النصب مالت الجوائز. النصابين عادة يطلبون 'رسوم' سريعة حتى يمشون المعاملة، وبعدين يختفون وماكو أي جائزة."
- **Critical limitation:** Google Cloud billing does not currently support Iraq. Multiple developer reports confirm Iraq is missing from the Cloud Billing country list, blocking standard API billing setup. Iraq is not on Google's published list of prohibited territories, and Google support has not disclosed a reason or a fix timeline. This makes Gemini's API impractical to depend on for a project built and demoed from Iraq, regardless of its dialect or pricing performance.

### 0.3.3 OpenAI's ChatGPT

- The best SDK/API, highly documented with a large community behind it.
- Excels at general-purpose tasks rather than specialized logic or assistance work.
- Pricing is similar to Claude's.
- ChatGPT was prompted to create a scenario and respond in Iraqi Arabic:
"أبو علي، انتبه، هاي أول مرة تحوّل لهذا الحساب والمبلغ أعلى من تحويلاتك المعتادة، فممكن تكون العملية مشبوهة. خل نراجعها قبل لا تأكد."
- Billing and account setup work normally from Iraq, with no access barrier found.

### Thoughts

**Claude**
- Logic and reasoning aren't essential for this use case
- Arabic output leans more Gulf-sounding (personal opinion)
- Pricing is on the higher end
- Billing and account access work without issue from Iraq
- **Verdict: not preferable**

**Gemini**
- Lowest price per token
- Best performance for assistance-style tasks
- Strongest Iraqi dialect in initial testing
- **Billing is not available from Iraq**, which is a practical blocker independent of model quality
- **Verdict: not viable for this project, despite strong dialect performance**

**ChatGPT**
- Easiest API to work with, given SDK maturity and documentation
- Initial dialect testing produced a somewhat vague response, but overall Iraqi Arabic output is workable
- Pricing is comparable to Claude's
- Billing and account access work without issue from Iraq
- **Verdict: preferable, primarily due to access reliability**

### Conclusion

**ChatGPT wins for the purposes of this hackathon.** While Gemini scored highest on cost and dialect quality in initial testing, Google Cloud billing is not currently available in Iraq, making it an unreliable foundation for a project that must be built, tested, and demoed from Iraq. ChatGPT's API offers dependable access, solid documentation, and Iraqi Arabic output sufficient for the coaching layer's needs, making it the more practical choice given this constraint.

# Stage 1: Research & Data 

**1.1 Detectable Cases:**

- New recipient anomaly — a transfer to a wallet account the user has never sent money to before, can be combined with other risk factors like Amount anomaly or Time-of-day anomaly.
- Amount anomaly: transaction size significantly outside the user's normal range (e.g., 10x+ their typical transfer, or their entire balance in one go).
- Time-of-day anomaly — a transaction at an hour the user has never transacted at before (e.g., 3 AM when their history is all daytime).
- Rapid sequence — multiple transfers within a short time window that break the user's normal cadence (e.g., 3 transfers in 5 minutes vs. their usual 1-2 per week).

**1.2 Data Required:**

- User profile: user ID, typical transaction frequency average , typical amount range average, typical hours of activity
- Transaction history: timestamp, amount, recipient ID, recipient is new/known flag
- Ground truth label: scam / not scam (For Demo Test purposes)

**1.3 Data Generation:**
- 40 Cases will be generated via ChatGPT and stored as JSON files in the Data folder
- of the 40 generated cases, 10 will be held out and not used during detection tuning 



