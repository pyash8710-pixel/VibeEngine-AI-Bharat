# VibeEngine AI - Requirements Document

## Project Overview

VibeEngine AI is a Viral Intelligence Ecosystem designed to empower content creators in Tier 2/3 Indian cities by transforming daily life experiences into engaging digital assets. The platform leverages AWS AI services to provide trend analysis, content optimization, and creator-business matchmaking.

## Target Users

- Content creators in Tier 2/3 Indian cities
- Small and medium-sized local businesses seeking digital presence
- Regional language content creators
- First-time creators lacking technical expertise

## User Stories

### For Content Creators

**US-1: Trend Discovery**
- As a creator, I want to understand what emotional triggers make content viral so that I can create relevant content
- Acceptance Criteria:
  - System analyzes YouTube trends using emotional categories (Nostalgia, Rage, Joy, etc.)
  - Results show top 10 trending topics with emotional breakdown
  - Regional filtering available (North, South, East, West India)

**US-2: Trend Saturation Warning**
- As a creator, I want to know if a trend is oversaturated so that I don't waste time on outdated content
- Acceptance Criteria:
  - Cringe Clock displays 0-100% saturation score
  - Visual indicator (Green: Fresh, Yellow: Moderate, Red: Oversaturated)
  - Historical trend curve showing saturation over time

**US-3: Regional Language Adaptation**
- As a creator, I want to convert my English scripts into regional slang so that my content resonates locally
- Acceptance Criteria:
  - Support for Bambaiyya, Hinglish, Tanglish, and other regional variants
  - Maintains original meaning while adding cultural flavor
  - Option to adjust slang intensity (Mild, Medium, Heavy)

**US-4: Presence Improvement**
- As a shy creator, I want feedback on my on-camera presence so that I can improve my delivery
- Acceptance Criteria:
  - Eye contact score (0-100%)
  - Vocal energy analysis (Low, Medium, High)
  - Frame-by-frame breakdown with improvement suggestions
  - Comparison with successful creators in similar niches

**US-5: WhatsApp-First Interaction**
- As a non-technical creator, I want to use WhatsApp to access all features so that I don't need to learn new apps
- Acceptance Criteria:
  - Upload video/audio via WhatsApp
  - Receive AI analysis within 2 minutes
  - Get actionable scripts and suggestions
  - Simple command structure (e.g., "analyze", "slang", "trends")

**US-6: Monetization Opportunities**
- As a creator, I want to connect with local sponsors so that I can monetize my content
- Acceptance Criteria:
  - Profile matching based on niche and audience demographics
  - List of relevant local businesses with contact details
  - Collaboration templates and pricing guidelines

**US-7: Multi-Language Content**
- As a creator, I want to dub my content in regional languages so that I can reach wider audiences
- Acceptance Criteria:
  - Support for 10+ Indian languages
  - Natural-sounding voiceovers
  - Sync with original video timing
  - Preview before final generation

### For Local Businesses

**US-8: Creator Discovery**
- As a local business, I want to find creators in my area so that I can sponsor relevant content
- Acceptance Criteria:
  - Search by location, niche, and audience size
  - View creator portfolios and engagement metrics
  - Direct contact mechanism

## Functional Requirements

### FR-1: Trend Pattern Analysis Engine

**FR-1.1: Data Collection**
- Integrate YouTube Data API v3 for trending video metadata
- Collect data from regional YouTube channels (minimum 1000 videos/day)
- Store video metadata: title, description, tags, views, likes, comments

**FR-1.2: Emotional Trigger Classification**
- Use Amazon Bedrock (Claude 3.5 Sonnet) to classify content into emotional categories
- Categories: Nostalgia, Rage, Joy, Surprise, Fear, Sadness, Inspiration
- Confidence score for each classification (0-100%)

**FR-1.3: Trend Reporting**
- Generate daily trend reports with top 10 topics
- Regional breakdown (state-level granularity)
- Exportable reports in PDF/JSON format

### FR-2: The Cringe Clock

**FR-2.1: Saturation Calculation**
- Track trend frequency over 30-day rolling window
- Calculate saturation score based on:
  - Upload frequency (videos per day)
  - Engagement decline rate
  - Creator diversity (unique channels covering trend)
- Formula: Saturation = (Current Frequency / Peak Frequency) × Engagement Decline Factor

**FR-2.2: Predictive Analytics**
- Forecast trend lifespan using historical patterns
- Alert creators 3 days before predicted saturation peak
- Suggest alternative angles for saturated trends

### FR-3: Slang Injector

**FR-3.1: Text Transformation**
- Use Amazon Bedrock (Llama 3) for slang conversion
- Support languages: Bambaiyya, Hinglish, Tanglish, Punglish, Benglish
- Preserve original sentence structure and meaning

**FR-3.2: Customization**
- Slang intensity slider (0-100%)
- Preserve specific words (brand names, technical terms)
- A/B comparison view (original vs. transformed)

### FR-4: Presence Auditor

**FR-4.1: Video Analysis**
- Use Amazon Rekognition Video for face detection and tracking
- Calculate eye contact percentage (gaze direction analysis)
- Measure facial expression consistency

**FR-4.2: Audio Analysis**
- Extract audio from video using FFmpeg
- Analyze vocal energy using amplitude and frequency analysis
- Detect filler words and pauses

**FR-4.3: Scoring and Feedback**
- Overall presence score (0-100)
- Detailed breakdown: Eye Contact (30%), Vocal Energy (30%), Expression (20%), Posture (20%)
- Timestamped suggestions for improvement

### FR-5: WhatsApp Bot Interface

**FR-5.1: Message Handling**
- Receive text, audio, video, and image messages
- Support commands: /analyze, /trends, /slang, /dub, /sponsors
- Session management for multi-step interactions

**FR-5.2: Media Processing**
- Download media from WhatsApp to S3
- Trigger appropriate Lambda functions based on media type
- Return results as text, images, or downloadable links

**FR-5.3: User Management**
- Register users on first interaction
- Store user preferences (language, niche, location)
- Track usage for analytics

### FR-6: Gully Sponsor Engine

**FR-6.1: Creator Profiles**
- Store creator data: niche, audience demographics, engagement rates, location
- Portfolio management (upload sample videos)
- Pricing and collaboration preferences

**FR-6.2: Business Profiles**
- Store business data: industry, budget, target audience, location
- Campaign requirements and objectives

**FR-6.3: Matching Algorithm**
- Score creator-business compatibility (0-100)
- Factors: Location proximity, audience overlap, budget alignment, niche relevance
- Generate top 5 matches for each query

### FR-7: Universal Voiceover

**FR-7.1: Language Support**
- Amazon Polly voices for: Hindi, Tamil, Telugu, Kannada, Malayalam, Bengali, Marathi, Gujarati, Punjabi, Odia
- Multiple voice options per language (male/female, age variations)

**FR-7.2: Dubbing Process**
- Extract original audio timeline
- Generate translated script using Amazon Translate + Bedrock
- Synthesize speech with timing synchronization
- Merge dubbed audio with original video

## Non-Functional Requirements

### NFR-1: Performance

- **Response Time**: WhatsApp bot responses within 2 minutes for video analysis
- **Throughput**: Support 1000 concurrent users
- **Trend Analysis**: Daily trend reports generated by 6 AM IST
- **Video Processing**: 1-minute video processed in under 60 seconds

### NFR-2: Scalability

- Auto-scaling Lambda functions based on demand
- DynamoDB on-demand capacity mode
- S3 lifecycle policies for cost optimization (archive after 90 days)
- CloudFront CDN for media delivery

### NFR-3: Reliability

- **Availability**: 99.5% uptime SLA
- **Data Durability**: S3 standard storage (99.999999999% durability)
- **Backup**: DynamoDB point-in-time recovery enabled
- **Error Handling**: Graceful degradation with user-friendly error messages

### NFR-4: Security

- **Authentication**: AWS IAM for service-to-service communication
- **Data Encryption**: At-rest (S3, DynamoDB) and in-transit (TLS 1.2+)
- **API Security**: API Gateway with API keys and rate limiting
- **Privacy**: User data anonymization for analytics
- **Compliance**: GDPR-compliant data handling

### NFR-5: Usability

- **WhatsApp Interface**: Natural language commands, no technical jargon
- **Response Clarity**: Actionable feedback with examples
- **Multilingual Support**: UI text in Hindi and English
- **Accessibility**: Text alternatives for all visual content

### NFR-6: Maintainability

- **Code Quality**: Minimum 80% test coverage
- **Documentation**: API documentation using OpenAPI 3.0
- **Monitoring**: CloudWatch dashboards for all services
- **Logging**: Structured logging with correlation IDs

### NFR-7: Cost Optimization

- **Budget**: Target $500/month for 1000 active users
- **Optimization Strategies**:
  - Lambda reserved concurrency for predictable workloads
  - S3 Intelligent-Tiering for media storage
  - DynamoDB on-demand for variable traffic
  - Bedrock batch processing for non-urgent requests

## Success Metrics

- **User Adoption**: 500 active creators in first 3 months
- **Engagement**: 60% weekly active user rate
- **Content Quality**: 30% improvement in presence scores after 5 uses
- **Monetization**: 100 creator-business connections in first quarter
- **Satisfaction**: Net Promoter Score (NPS) > 50

## Out of Scope (Phase 1)

- Mobile native apps (iOS/Android)
- Live streaming analysis
- Automated video editing
- Payment processing for sponsorships
- Creator community features (forums, groups)
