# Changelog

All notable changes to this project will be documented in this file.

## v0.1.0 — 2026-09-07

### Added

- Initial project repository structure
- Initial project documentation structure
- Initial system architecture
- Initial technology stack definition
- Initial Brand Bible concept
- Initial Content Object concept
- Initial QA concept
- Initial knowledge domains for medical, history, research, safety, and trends
- Initial content production structure for trends, recommendations, topics, scripts, claims, scenes, and prompts
- Initial QA documentation structure
- Initial workflow structure for n8n and Dify
- Initial data schema structure for trends, recommendations, content, scenes, schedules, and QA results
- Initial integration documentation structure for external AI, generation, rendering, TTS, and publishing services
- GitHub repository configuration

### Decisions

- n8n selected as the orchestration layer
- Dify selected as the AI content brain
- Creatomate selected as the initial rendering candidate
- fal.ai selected as the initial generation candidate
- ElevenLabs selected as the initial TTS candidate
- YouTube Data API selected as the initial publishing interface

### Notes

- Detailed architectural and technology decisions are maintained in `05_decisions/`.
- The project repository structure is intended to evolve as the AI Factory architecture is validated through experiments.

## v0.1.1 — 2026-09-09 ~

> Repository structure 확립

### Added

- Expanded the project repository structure
- Added architecture documentation for:
  - Data architecture
  - Internal protocol
  - State machine

- Added Brand documentation structure:
  - Brand Bible
  - Visual style
  - Content format

- Added knowledge domains:
  - Medical
  - History
  - Research
  - Safety
  - Trends

- Added decision management documents:
  - Decision log
  - Option register
  - Rejected ideas

- Added experiment documentation
- Added content production structure:
  - Trends
  - Recommendations
  - Topics
  - Scripts
  - Claims
  - Scenes
  - Prompts

- Added QA documentation:
  - QA protocol
  - Trend QA
  - Medical QA
  - Historical QA
  - Video QA

- Added workflow structure for n8n and Dify
- Added data schemas for:
  - Trend objects
  - Topic recommendations
  - Content objects
  - Scene objects
  - Schedule objects
  - QA results

- Added integration documentation for:
  - Trend sources
  - OpenAI
  - fal.ai
  - ElevenLabs
  - Creatomate
  - YouTube

- Added GitHub repository configuration under `.github/`

### Notes

- Existing decision records were preserved:
  - `05_decisions/D-000-Template.md`
  - `05_decisions/D-004-creatomate-rendering.md`

- Existing `10_schemas/content-object.json` was preserved.
- The expanded repository structure establishes the initial foundation for the AI content production pipeline.
