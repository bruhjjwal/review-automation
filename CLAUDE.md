# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an n8n workflow automation project for review moderation. The repository contains a single n8n workflow configuration file that implements an AI-powered review analysis system for Bewakoof fashion brand.

## Architecture

The system is built as an n8n workflow that processes customer reviews through multiple stages:

### Core Workflow Flow
1. **Cron Trigger** → Scheduled execution
2. **Fetch Pending Reviews** → API call to retrieve pending reviews from Bewakoof API
3. **Data Preparation** → JavaScript processing to clean and structure review data
4. **Split In Batches** → Process reviews in batches of 5
5. **Review Router** → Route reviews based on image presence:
   - **Image Path**: Product image fetching → Image similarity analysis → Decision logic
   - **Text Path**: Direct text analysis
6. **AI Analysis** → Google Gemini API calls for content analysis
7. **Final Decision Switch** → Route to appropriate action (approve/reject/manual review)
8. **Dashboard Updates** → API calls to update review status in Bewakoof system

### Key Components

**Review Processing Logic (`Review Automation (1).json`)**:
- Image similarity analysis using Google Gemini Vision API
- Text sentiment analysis using Google Gemini Flash API
- Multi-stage decision making with configurable thresholds
- Automatic routing based on confidence scores

**API Integrations**:
- Bewakoof API endpoints for fetching and updating reviews
- Google Generative AI API for image and text analysis
- AWS S3 for product image storage

**Decision Criteria**:
- Image similarity threshold: 50/100 minimum
- Text sentiment threshold: 50/100 minimum
- Automatic approval/rejection with manual review fallback

## Configuration Requirements

Before deploying this workflow, ensure these values are configured:

1. **API Keys**:
   - `YOUR_GOOGLE_API_KEY` - Google Generative AI API key
   - Bewakoof API authentication (headers/tokens)

2. **AWS Configuration**:
   - `YOUR_BUCKET` - S3 bucket name for product images

3. **Thresholds** (configurable in code):
   - Image similarity threshold (default: 50)
   - Text sentiment threshold (default: 50)

## Workflow Execution

The workflow runs on a cron schedule and processes reviews automatically. Manual execution is also supported through n8n interface.

**Processing Flow**:
- Fetches up to 5 pending reviews per execution
- Processes each review individually through AI analysis
- Updates review status via API calls to Bewakoof dashboard
- Handles errors gracefully with manual review fallback

## Error Handling

The workflow includes comprehensive error handling:
- API timeout handling (30-60 second timeouts)
- JSON parsing error recovery
- Image processing failure fallbacks
- Automatic routing to manual review for edge cases

## Development Notes

- This is an n8n workflow file, not traditional source code
- Modifications should be made through n8n workflow editor
- Test with small batches before production deployment
- Monitor API rate limits for Google Generative AI
- Ensure proper authentication for all external APIs

## Security Considerations

- API keys are stored as workflow parameters (should use n8n credentials)
- Review content is processed through external AI services
- All HTTP requests use HTTPS
- Sensitive data flows through multiple external services