---
name: token-estimator
description: Predicts and calculates token usage for UXPilot AI features based on input volume and configuration.
---
# Token Usage Estimator
This skill helps you calculate and predict the prompt and output token usage for different features in the UXPilot application. When the user asks for token predictions or budget calculations, apply the following rules to provide an accurate estimate.
## Token Value Baselines
* **Text**: 1 Token ≈ 4 characters of English text (~0.75 words).
* **Images**: 
  - **Gemini**: Fixed cost of ~258 tokens per image/page regardless of size.
  - **OpenAI/Anthropic**: Base 85 tokens + 170 tokens for each 512x512 tile (approx. 1,100 tokens per 1080p image/page).
---
## Feature Cost Rules & Formulas
### 1. Expert UX Audit (Design Review)
Reviewing UX designs based on heuristics and specific personas.
- **Base Setup**: 500 tokens (System Instructions + Dynamic Constraints)
- **Input Variables**: Number of pages/images.
- **Output Variables**: ~1,500 tokens (Detailed JSON with multiple friction points).
- **Calculation Formula**: `500 + (Pages × ImageTokenCost) + 1500`
- **Example**: Reviewing 5 pages of a UX design on Gemini: 
  `500 + (5 × 260) + 1500 = ~3,300 tokens`.
### 2. Design Comparison (A/B Audit)
Comparing two or more variations against each other.
- **Base Setup**: 600 tokens (System Rules + Matrix Directives)
- **Input Variables**: Total number of alternative pages/images submitted.
- **Output Variables**: ~2,500 tokens (Includes individual evaluations + comparative reasoning scorecards).
- **Calculation Formula**: `600 + (TotalPages × ImageTokenCost) + 2500`
- **Example**: Comparing Version A to Version B (2 pages) on OpenAI:
  `600 + (2 × 1100) + 2500 = ~5,300 tokens`.
### 3. Persona Builder & Simulated Agents
Building logic structures from data, or injecting context to configure an agent.
- **Base Setup**: 200 tokens
- **Input Variables**: Uploaded evidence size (Word count / 0.75 = Text Tokens).
- **Output Variables**: ~500 tokens (Structured JSON configuration).
- **Calculation Formula**: `200 + EvidenceTextTokens + 500`
- **Example**: Synthesizing a persona from a 1,500-word user interview:
  `200 + (1500 / 0.75) + 500 = ~2,700 tokens`.
### 4. Quick Survey Data Processing
Synthesizing user research data into clear segment patterns.
- **Base Setup**: 300 tokens
- **Input Variables**: Volume of unstructured survey data.
- **Output Variables**: ~2,000 tokens.
- **Calculation Formula**: `300 + SurveyDataTokens + 2000`
- **Example**: Processing 100 open-text survey responses (~3,000 words):
  `300 + (3000 / 0.75) + 2000 = ~6,300 tokens`.
---
## Agentic Instructions for Applying this Skill
If the user asks "How many tokens will feature X use?":
1. First, identify the **Feature** (Audit, Comparison, Persona, Survey).
2. Ask the user (or safely assume standard defaults) regarding the **Inputs** (Ask: "How many pages are you auditing?" or "How many words of survey data do you have?").
3. Ask which **AI Provider** they prefer to baseline against, or default to their primary system (Gemini 2.0 Flash is currently standard).
4. Perform the mathematical calculation using the formulas above.
5. Provide a clear breakdown of **Input Tokens**, **Output Tokens**, and **Total Tokens**.
