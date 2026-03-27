---
layout: post
title:  "What's Old is New Again: The Quiet Burst of the AI Hype Bubble "
date:   2026-03-26 10:00:00 +0600
categories: tech
---

Time is a flat circle. At least, that's what it feels like when you've been in technology long enough to watch the same story play out over and over again with different buzzwords and weird company names. It's a bad time to be a neurodivergent patter-recognizer.

Let's hop in the (slightly) way back machine to the early days of cloud computing. The pitch was intoxicating: stop spending capital on servers, stop maintaining expensive operations teams, stop worrying about capacity planning. Just hand it all to AWS or Azure or GCP and pay for what you use. Convenience, scale, and someone else's problem. Organizations stampeded toward managed services: databases, message queues, identity, observability, and anything else they could possibly offload the operational complexity to in exchange for a monthly invoice that seemed, at first, entirely reasonable.

Then interest rates went up. Money got expensive. CFOs started scrutinizing line items they'd been waving through for years. And what those organizations discovered was that the operational costs they'd traded away had quietly transformed into something more insidious: structural dependencies on third-party pricing decisions they had absolutely no influence over. Lock-in had arrived cosplaying as convenience. The savings from not running your own infrastructure had evaporated, and in many cases the cloud bill had grown to dwarf what the old data center would have cost.

Mind you, this wasn't a complete reckoning and turning away. As we know cloud still remains the right answer for many workloads, and the industry learned from it. Modern containerization and orchestration tools like Kubernetes made it genuinely feasible to build workloads that could run in a public cloud, a private cloud, or on-premises hardware without being rewritten from scratch. A hybrid posture stopped being an architectural compromise and became a legitimate operational strategy. Organizations could now chase cost and performance across environments with more flexibility than the original cloud migration promises ever delivered.

But the lesson had been learned the hard way: all that glittered was not gold when it came to trusting critical infrastructure and services entirely to third parties. Companies realized, as Mordo admonished Dr. Strange, that "The bill comes due, always."

## Yet Another Remake

People complain that Hollywood has run out of ideas and that every decent (or not so decent) movies is just the first of a budding franchise. Hell, there's another Scream movie coming out (it doesn't matter when you read this, it will still be true). Just like that deja vu feeling when you watch the movie previews, we are also feeling it when we are looking at some of these financials behind the use of GenAI. Just like when those AWS cloud adoption credits ran out and the first *real* bills started to roll in, organizations are starting to feel that particular kind of anxiety that sets in when you can't tell what something is going to cost next month. This time, though, it's their AI bill. It's not just the cost itself it's the *not knowing*. And that's precisely what API-driven LLM inference has become for a growing number of teams: a variable that keeps surprising people on the wrong side.

API pricing models have some deeply uncomfortable properties. Your bill scales with usage, but usage is hard to predict, especially once you embed AI into products and pipelines where it gets called automatically. It could be a runaway agentic workflow, a context window that bloated when nobody was watching, a productive week where your developers leaned hard on their coding assistants or whatever. Any of these can turn a reasonable estimate into an invoice you have to explain to finance. And the emerging class of reasoning-heavy models has made this worse, not better. Models that think longer produce better outputs, but they're also burning tokens while they do it, and those tokens are expensive.

The result? A growing wave of organizations, and not all of them, and not for every use case, but a meaningful and accelerating number are making deliberate decisions to move inference workloads off external APIs and onto infrastructure they own and operate. Cloud instances with reserved pricing, on-premises GPU hardware, hybrid approaches where some models live inside the network and others don't. The specific mechanism varies but the underlying motivation is consistent: they want a number they can plan around.

This isn't technophobia or a rejection of AI. It's the same operational discipline that cloud bill-burned organizations applied to their infrastructure decisions just showing up a few years later, in a new domain.

## Aww, These Small Models Are So Cute!

While the infrastructure conversation has been developing, something else was quietly happening on the model side. The open-weight model ecosystem matured, and _fast_. The performance gap between a general-purpose frontier LLM and a smaller, well-tuned model narrowed dramatically for tasks with any real specificity to them. A 7B model fine-tuned on your support ticket history will handle your support tickets better than GPT-5 will, while costing a fraction of the compute and running on hardware you can actually afford to own.

[Gartner has projected](https://www.gartner.com/en/newsroom/press-releases/2025-04-09-gartner-predicts-by-2027-organizations-will-use-small-task-specific-ai-models-three-times-more-than-general-purpose-large-language-models) that by 2027, organizations will deploy task-specific small models three times more than general-purpose LLMs. That's a structural shift, not a blip. And it's being driven by something that sounds almost mundane when you say it out loud: smaller, specialized models are often just more accurate for the things enterprises actually need to do most.

The reason isn't mysterious. A large general model has to be good at everything, which means it's optimized for breadth. It has seen the entire internet and needs to generalize across it. A fine-tuned small model trained on legal contract language, or medical coding guidelines, or your company's internal documentation, doesn't have to know anything else. It knows its domain deeply and nothing outside it. That focus, combined with dramatically better open-weight base models as starting points, means you can get genuinely good results from a single GPU using a model you own, control, and can modify.

Fine-tuning one of these models isn't the research project it used to be. With 1,000 to 5,000 domain-specific examples and a few weeks of work, you have something that runs on hardware costing less than a decent server, responds in milliseconds because there's no network round-trip, and improves as you feed it more of your own data.

## Why These Two Trends Play Nicely Together

These aren't parallel developments that happen to be occurring simultaneously. They're solving each other's problems. They're simpatico. They're synchronized swimming. They're getting their self-hosting peanut butter into my small language model chocolate.

The concern with self-hosting large models has always been that the infrastructure cost is significant. You're not just paying for the GPU, you're paying for it whether your model is busy or idle, and a 70B parameter model requires enough hardware that "whether you use it or not" is a meaningful consideration. The economics only work if your utilization is high and your alternative (the API bill) was already expensive.

Small models change that math entirely. A 7B model serving specialized tasks runs on a single GPU. A 13B model fits comfortably in 16GB of VRAM. A well-quantized 32B coding model runs on 24GB which is hardware available in a single consumer-grade server. The infrastructure cost floor drops from "significant investment requiring executive sign-off" to "a line item on an engineering team's budget." And once the monthly infrastructure cost is modest and fixed, the "fixed cost regardless of usage" property that seems like a downside suddenly becomes exactly the feature you wanted: a known, predictable number.

The privacy story sharpens too. Sending prompts to an external API means that data, whatever it contains, is traveling to infrastructure you don't control. For regulated industries, that's often a non-starter regardless of what the vendor's data processing agreement says. Self-hosting solves that. But self-hosting a large model historically required significant infrastructure that might itself introduce new complexity and surface area. A small model running on a single modest server inside your network introduces none of that. The data doesn't leave, the infrastructure doesn't require a team to manage, and the compliance answer is simple.

There's a customization dimension too, and it matters more than it many might thing. A fine-tuned model trained on your proprietary data isn't just marginally better at your tasks, it's dramatically better because it encodes knowledge that nobody else has. Your workflows, your terminology, your edge cases, the specific failure modes that matter in your domain. Over time, with continued fine-tuning, that gap widens. A generic LLM accessed via API will always reflect its training distribution, which is the broader internet plus whatever RLHF shaped it. A small model trained on your data becomes, in a very real sense, yours: an asset that compounds in value as you put more into it.

None of this requires you to abandon frontier models for everything. The smart architectural move emerging in 2026 is tiered: a lightweight router examines incoming requests, sends the well-defined, high-volume tasks to a self-hosted specialized model, and escalates genuinely novel or complex reasoning to a frontier model when the task actually warrants it. You pay the variable API cost only when nothing else can handle the job. For most production workloads, that turns out to be a small minority of traffic.

This method isn't just vaporware or conjecture. We are designing and building these architectures for customers routinely. There is a sizeable market already for both consumer and enterprise small language models to do various tasks and power agents and we are using the same infrastructural tools and patterns to train and host them.

## Does The Shoe Fit Me, Though?

There's a practical question buried in all of this: when does it make sense to move in this direction, and what does that actually look like?

The cost case becomes compelling when you have high-volume, repetitive tasks where you're paying per-token and can see what that's adding up to. Customer support triage. Document classification. Internal search and retrieval. Code completion for a specific stack. These are tasks where the behavioral requirements are well-understood, the training data exists, and the same operation is being performed thousands of times. That's exactly where a fine-tuned small model earns back its setup cost in a matter of months.

The privacy case is often immediate. If your workloads involve customer PII, proprietary business data, medical records, or anything else you'd be uncomfortable sending to an external provider, the conversation is less about economics and more about whether you can use cloud APIs at all. For many organizations in healthcare, finance, and legal services, self-hosted models aren't a cost optimization, they're the only path to deployment.

The customization case is a longer-term play, but it's worth thinking about early. The organizations building proprietary fine-tuned models today are accumulating an asset. Three years from now, a model that has been continuously improved on your operational data will be genuinely difficult for a competitor to replicate by calling the same API you both have access to. That competitive angle is underappreciated in the current conversation, which tends to focus on cost and privacy.

What this doesn't mean is that every organization should immediately rip out their API integrations. We are talking more about the cloud maturity model applied to AI. For early-stage products, for exploration, for tasks requiring genuine generality, API-based access to frontier models remains the fastest path. The point isn't to never use external APIs. The point is to make deliberate choices about which workloads belong where and to recognize that the same concerns that are driving the conversation about self-hosting are also the ones that make small, specialized models attractive. They're not two separate decisions. They're one decision with two complementary answers.

There's also a level of organizational maturity that comes along with this. As many companies learned the hard way, there is a scale at which not having an operations group becomes a liability rather than an optimization. If you don't have the operational knowledge or bandwidth to pivot to a hybrid approach, you're at the will of the API vendors. It pays to develop and maintain the ability to run some aspects of your own infrastructure at a certain point and it's best to do so _before_ you're at that point.

## A Different Kind of Progress

There's something worth noting about where this leaves us. The dominant narrative around AI has been that bigger is better, that the frontier model is always the right answer, and that the future belongs to whoever has access to the most capable general system. That narrative served a particular moment in the technology's development, when model quality varied wildly and the small models genuinely couldn't compete.

We're not in that moment anymore.

The progress happening at the smaller end of the model spectrum — better training data, distillation from larger models, improved quantization, architectures that do more with less — means that "fit for purpose" is now a realistic design goal rather than a consolation prize. A 3B parameter model trained on the right data can outperform GPT-5 on the specific task it was designed for. That's not spin. That's what the benchmarks show when you test on domain-specific tasks instead of general capability tests.

The infrastructure decisions following from this aren't about rejecting innovation. They're about applying the same engineering discipline to AI workloads that we've applied to every other part of the stack: use the right tool for the job, own the parts of your system that give you competitive advantage, and don't let your cost structure be somebody else's decision to make.

That's not a particularly revolutionary idea. It's just good engineering, applied to a space that's been moving too fast for most organizations to think carefully about it. AI allows us to screw up faster than ever, but if you listen to  those of us who've seen this movie before, we can help you avoid the jump scares.

