---
inclusion: manual
---

# AI Monthly Newsletter Blog Post Generation

When triggered to generate the monthly AI newsletter blog post, follow this process:

## Sources to Check

Fetch and review the latest content (past 30 days) from these sources:

1. Anthropic Blog - https://www.anthropic.com/news
2. OpenAI Blog - https://openai.com/blog
3. Google DeepMind Blog - https://deepmind.google/discover/blog/
4. Hacker News (AI-tagged) - https://news.ycombinator.com (search for AI/LLM topics)
5. arXiv CS.AI - https://arxiv.org/list/cs.AI/recent
6. The Verge AI - https://www.theverge.com/ai-artificial-intelligence
7. TechCrunch AI - https://techcrunch.com/category/artificial-intelligence/
8. MarkTechPost - https://www.marktechpost.com
9. Towards Data Science - https://towardsdatascience.com/

## Writing Guidelines

- Write in first person as Anup Katariya, an engineering director who builds with AI tools
- Be opinionated. Take positions on what matters and what doesn't
- Raise 2-3 open questions at the end that don't have clear answers yet
- Keep it to 1000-1500 words
- Group by theme, not by source (don't just list articles)
- Skip hype. Focus on what's actually changing for practitioners
- Reference the tropes.md steering file for writing style (avoid AI writing patterns)
- Use Chirpy front matter format with categories: [AI] and relevant tags

## Output Format

Save as `_drafts/YYYY-MM-DD-ai-monthly-<month>.md` with proper Chirpy front matter.

## After Generation

Tell the user:
1. The draft is ready at the path
2. They should review and edit
3. When ready, move to `_posts/` and commit/push
