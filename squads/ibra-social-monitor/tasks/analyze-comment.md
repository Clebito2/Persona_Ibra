# Task: Analyze Social Media Comment

## Description
Extract data and context from a social media comment and its associated profile.

## Elicitation
elicit: true
format: |
  Post Context (Caption/Subject): [Insert description]
  Comments List:
    - [Comentador 1]: [Texto do Comentário]
    - [Comentador 2]: [Texto do Comentário]

## Instructions
1. **Data Extraction & Web Access**:
   - Use `exa` or `read_url_content` to fetch full caption and relevant high-engagement comments.
   - Identify the primary intent (question, critique, praise, provocation).
   - Flag micro-sentiments (sarcasm vs. genuine doubt).
2. **Visual Profiling**:
   - Analyze profile photo for professional/social cues (age estimate, profession, ideological badges).
   - Infer the persona level (Influencer, Bot, Casual User, Intellectual Critic).
3. **Sentiment & Theory Check**:
   - Map the comment to broader social theories if applicable (delegated to Analyst output).

## Artifacts
- **type**: deep_comment_analysis
- **format**: json
- **fields**: [intent, micro_sentiment, profile_visual_cues, commenter_persona, extracted_content]
