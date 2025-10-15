---
title: "Three months ago, a CTO told me his team couldn't move faster without hiring. Last week, they shipped more in one sprint than they did all quarter."
date: 2025-01-15
draft: false
---

Three months ago, a CTO told me his team couldn't move faster without hiring. Last week, they shipped more in one sprint than they did all quarter.

Same people. Same codebase. The only thing that changed was how they worked.

I met this CTO at a founder event in Amsterdam. His company had raised Series A and was growing, but the engineering team was struggling to keep up. They had twelve engineers and were planning to hire six more.

When I asked what would change with eighteen engineers, he said they'd work on more features in parallel. But when I asked how long features were taking, something interesting emerged.

Features weren't taking long to build. They were taking long to ship. Code sat in review for days. Deployments happened once a week. Testing was manual. Engineers were constantly blocked waiting for something.

The bottleneck wasn't coding capacity. It was the system around the coding.

I proposed an experiment: spend three months removing friction from their process instead of hiring. If it worked, they'd ship faster. If not, hire later. He agreed.

We made three changes. First, we automated deployment. What took two hours and three people now takes five minutes automatically. Second, we introduced AI-assisted code review to catch obvious issues before human review. Third, we built simple N8N automations connecting their tools.

The technical work took maybe forty hours. The hard part was convincing the team these changes wouldn't compromise quality.

In the first month, deployment frequency went from once a week to multiple times per day. Bugs got fixed in hours instead of waiting for deployment windows.

By month two, they shipped features in one sprint that previously took three. Not from working harder, but because latency between steps had collapsed.

By month three, they'd shipped their entire quarter's roadmap. The CTO called me: "We're not hiring those six engineers. We don't need them anymore."

I tell this story because we often misdiagnose our problems. When a team is slow, the instinct is to add more people. But that often makes things worse. More people means more coordination overhead.

The real question is: where is your team spending their time? If it's writing code, you need more engineers. But if it's waiting—for review, deployment, approvals, decisions—then adding people won't help. You need to remove the waiting.

When that team measured their lead time from commit to production, it averaged nine days. After our changes, it dropped to under two hours. Not because engineers got better at coding. Because we removed eight days of waiting.

I've now worked with about a dozen teams on similar transformations. The constraint is almost never coding capacity. It's almost always process latency and coordination overhead.

If you're considering hiring to speed up your team, first measure where time actually goes. Track how long code sits in review. Track merge to production time. Track how much time engineers spend waiting versus building.

You might find the constraint isn't people. It's process. And adding more people will just make it worse.

I work with teams through hands-on training over four sessions. We examine workflows, identify bottlenecks, and build automation to eliminate them. Most teams see measurable improvements within thirty days.

The companies that win in 2025 won't be the ones with the most engineers. They'll be the ones who eliminated friction from their development process.

---

Let's talk: https://calendly.com/nasir-fio/30min
