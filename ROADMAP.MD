Karar Hayder's entry into the zaincash agentic AI competition 9/22/2026
Below is my full roadmap and thought process detailed

## Stage 0: Local vs API

This is a Matter of cost, reliability and Maintainability,
We need Low Cost, High reliability and High Maintainability, all while being scaleable for increasing amounts of customers.

**Self hosted LLM Server**

According to blogs, Local AI GPU hardware for 70B+ models typically costs $4,200 for a professional RTX A6000 or 
$10,000+ for an A100 with a passive cost of $30 – $80+ per month due to electricity,
all while putting fees for physical maintainance aside with scaleability being a roadblock.

Pros: Completely private enviorment for privacy , Fully tailored for specific needs.

Cons: Needs a full trained team to maintain and work on,
infastructure is limited and scaleability can put the service integrity at risk,
Needs infastructure for backups incase something goes wrong with the main server
which would be double cost.

Conclusion: Not preferable.


Cloud based LLM-

For starters, this provides the Scaleable infastructure with possiblity of backups
The Main provider would be Amazon as they have many services such as AWS and even services more tailored towards AI.

Three Main Amazon services to note,

Bedrock API
-Multiple AI models readily avaliable
-Zero infastructure management
-Highly controlable LLMs through Guardrails and knowledge bases

SageMaker Endpoints
-Can Use Custom fine tuned LLMs
-More infastructure management, better control
-Predictable token usage

Self-Hosted EC2
-Handles High token volume >10B monthly
-requires a Machine learning team to manage the infastructure
-Has custom inference optimizations (vLLM, TensorRT-LLM, custom kernels)
-Can have 1-3 year Reserved Instances for the deepest discounts

Conclusion:

this has high maintainablity, reliabilty (Backup LLM models when one fails), with cost varying.

| Option | Cost | Best at |
|---|---|---|
| Bedrock API | $0.22–0.44 per 1M tokens (7B model) | <10M tokens/month |
| SageMaker (ml.g5.xlarge) | ~$1,015/month | 10–50M tokens/month |
| Self-hosted EC2 (g5.xlarge) | $724/mo on-demand, $290/mo reserved | >50M tokens/month |

For the hackathon purposes this is not preferable,
but on a larger scale Could be used based on company needs.

Source: wring.co/blog/aws-llm-hosting-vs-api-costs, 2026.

Direct API LLM-

This method is using direct API calls to AI providers with no middle man,
Easily maintainable with reliability and cost varying based on the provider.

Anthropic's Claude

-Claude's main selling point is logic, tasks such as coding or work
-Varied pricing based on Tiers, Flagship-tier pricing in the roughly $5/$25 per million input/output tokens range with cheaper tiers avaliable.
-Claude was prompted to make a scenario and speak in iraqi arabic-
replied with the following:
"وكفة شوي — هاي أول مرة ترسل فلوس لهذا الحساب، والمبلغ أكبر من المعتاد بكثير. إذا وصلتك رسالة تگول فزت بجائزة وتحتاج ترسل فلوس حتى تستلمها، هاي طريقة نصب معروفة. الشركات الحقيقية ما تطلب منك ترسل فلوس عشان تستلم جائزة. تريد تكمل العملية؟"

Google's Gemini

-cheapest per-token option among the three
-Very peformant on Assistance rather than logical work (such as claude's coding)
-Gemini was prompted to make a scenario and speak in iraqi arabic-
"عمي، دير بالك. هاي أول مرة تحول فلوس لهذا الرقم، والقصة تشبه سوالف النصب مالت الجوائز. النصابين عادة يطلبون 'رسوم' سريعة حتى يمشون المعاملة، وبعدين يختفون وماكو أي جائزة."

OpenAI's ChatGPT

-The best SDK/API, highly documented with a large community behind it.
-excels at general purpose tasks rather than specialised logic or assistance
-Similar pricing to claude
-Chatgpt was prompted to make a scenario and speak in iraqi arabic-
"أبو علي، انتبه، هاي أول مرة تحوّل لهذا الحساب والمبلغ أعلى من تحويلاتك المعتادة، فممكن تكون العملية مشبوهة. خل نراجعها قبل لا تأكد."

Thoughts:

-Claude's logic and reasoning isnt all that needed here
-Claude's arabic is more of gulf sounding (personal opinion)
-Claude's pricing is on the higher end

-Claude is not preferable

-ChatGPT's API would be the easiest to work with
-ChatGPT kept being vague in the prompt reply
-ChatGPT's pricing is similar to claude
-ChatGPT's iraqi arabic is better than claude

-ChatGPT is more preferable

-Gemini has the lower prices
-Gemini is better for assistance
-Gemini has the best iraqi dialect 

-Gemini Beats ChatGPT

Conclusion:
Gemini wins for the purposes of this hackathon,
lowest cost per token - strongest Iraqi dialect - Good reasoning for an AI assistant rather than a full blown cybersecurity expert.







