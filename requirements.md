# Requirements Document: SarkarSahayak AI

## Introduction

SarkarSahayak AI is a voice-first assistant designed to empower rural and underserved Indian communities by simplifying access to government schemes, public services, and local resources through natural conversations in multiple Indian languages. The system addresses the critical barrier faced by 600M+ rural Indians who struggle to access government schemes due to low literacy, language barriers, and limited internet connectivity.

## Glossary

- **Voice_Interface**: The WhatsApp Business API and Twilio IVR system that accepts voice input from users
- **Speech_Processor**: The AWS Polly, Google Speech-to-Text, and Hugging Face Indic models that convert speech to text and text to speech
- **NLP_Engine**: AWS Lex multilingual natural language processing system
- **API_Gateway**: AWS Lambda functions that integrate with government APIs
- **Government_API**: External APIs including DigiLocker, UMANG, and other government data sources
- **User_Profile**: DynamoDB stored user preferences and interaction history
- **Delivery_System**: The multi-channel delivery mechanism (WhatsApp, IVR, SMS)
- **Regional_Language**: Any of the 10+ supported Indian languages including Hindi, Tamil, Telugu, etc.
- **Scheme**: Government welfare programs such as PM Awas, Ayushman Bharat, PM Kisan
- **Offline_Mode**: SMS fallback and zero-data operation mode
- **Community_Tool**: Features for complaint filing, rights explanation, and group sharing

## Requirements

### Requirement 1: Multilingual Voice Input Processing

**User Story:** As a rural user with low literacy, I want to speak my query in my regional language, so that I can access government services without needing to read or type.

#### Acceptance Criteria

1. WHEN a user initiates a voice query via WhatsApp call or IVR, THE Voice_Interface SHALL accept audio input in any of the 10+ supported Regional_Languages
2. WHEN audio input is received, THE Speech_Processor SHALL convert speech to text with minimum 90% accuracy for rural dialects
3. WHEN speech-to-text conversion fails, THE System SHALL prompt the user to repeat their query with clearer audio guidance
4. WHEN the Regional_Language is detected, THE System SHALL maintain that language context throughout the conversation session
5. THE Speech_Processor SHALL process audio input within 3 seconds of receiving the complete voice query

### Requirement 2: Government Scheme Eligibility Query

**User Story:** As a rural citizen, I want to check my eligibility for government schemes by speaking naturally, so that I can understand which benefits I qualify for.

#### Acceptance Criteria

1. WHEN a user queries about scheme eligibility, THE NLP_Engine SHALL identify the specific Scheme being referenced (PM Awas, Ayushman Bharat, PM Kisan, etc.)
2. WHEN scheme identification is complete, THE API_Gateway SHALL query the relevant Government_API with user parameters
3. WHEN eligibility data is retrieved, THE System SHALL determine user qualification based on government criteria
4. WHEN multiple schemes match the query, THE System SHALL present all relevant schemes in order of eligibility likelihood
5. WHEN a user is eligible for a Scheme, THE System SHALL provide step-by-step enrollment instructions in audio format
6. WHEN a user is not eligible, THE System SHALL explain the reasons and suggest alternative schemes

### Requirement 3: Personalized Resource Discovery

**User Story:** As a rural user, I want to find nearby clinics, jobs, and markets using voice commands, so that I can access local resources without navigating complex apps.

#### Acceptance Criteria

1. WHEN a user requests nearby resources (clinics, jobs, markets), THE System SHALL identify the resource type and user location
2. WHEN location is unavailable, THE System SHALL prompt the user to provide their village or district name via voice
3. WHEN resource search is performed, THE API_Gateway SHALL query location-based databases and return results within 5km radius
4. WHEN results are found, THE Delivery_System SHALL provide audio directions with landmark references familiar to rural users
5. WHEN maps are available, THE System SHALL send visual maps via WhatsApp with audio description
6. WHEN no resources are found within 5km, THE System SHALL expand search radius to 20km and inform the user

### Requirement 4: Voice Response Generation

**User Story:** As a user with limited reading ability, I want to receive responses in audio format in my language, so that I can understand the information without reading text.

#### Acceptance Criteria

1. WHEN a response is generated, THE Speech_Processor SHALL convert text to natural-sounding speech in the user's Regional_Language
2. WHEN audio response exceeds 60 seconds, THE System SHALL break it into digestible segments with pause prompts
3. WHEN technical terms are present, THE System SHALL use simplified local language equivalents
4. WHEN the user requests repetition, THE System SHALL replay the last audio segment at slower speed
5. THE Speech_Processor SHALL generate audio responses within 2 seconds of text preparation

### Requirement 5: Offline and SMS Fallback

**User Story:** As a user in an area with poor internet connectivity, I want to access services via SMS when voice fails, so that I can still get information despite network limitations.

#### Acceptance Criteria

1. WHEN voice connection fails or is unavailable, THE System SHALL automatically switch to SMS-based interaction mode
2. WHEN in Offline_Mode, THE System SHALL accept text queries in Regional_Language via SMS
3. WHEN SMS query is received, THE System SHALL process it using the same NLP_Engine and return concise text responses
4. WHEN response exceeds SMS character limit, THE System SHALL split into multiple sequential messages
5. THE System SHALL maintain conversation context across multiple SMS exchanges within a 30-minute session
6. WHEN internet connectivity is restored, THE System SHALL offer to switch back to voice mode

### Requirement 6: Community Complaint Filing

**User Story:** As a citizen experiencing issues with public services, I want to file complaints via voice, so that I can report problems without visiting government offices.

#### Acceptance Criteria

1. WHEN a user initiates complaint filing, THE System SHALL collect complaint details via voice prompts (issue type, location, description)
2. WHEN complaint details are complete, THE System SHALL generate a unique complaint reference number
3. WHEN complaint is submitted, THE API_Gateway SHALL transmit it to the appropriate Government_API endpoint
4. WHEN submission is successful, THE System SHALL send confirmation via audio and SMS with the reference number
5. WHEN a user provides a reference number, THE System SHALL retrieve and communicate complaint status
6. THE System SHALL allow users to add photo evidence via WhatsApp image upload

### Requirement 7: Rights and Entitlements Explainer

**User Story:** As a rural citizen, I want to understand my legal rights and entitlements in simple language, so that I can be aware of what the government provides.

#### Acceptance Criteria

1. WHEN a user asks about rights or entitlements, THE NLP_Engine SHALL identify the specific right category (labor, land, welfare, etc.)
2. WHEN the category is identified, THE System SHALL retrieve relevant rights information from the knowledge base
3. WHEN explaining rights, THE System SHALL use simple language with real-world examples relevant to rural contexts
4. WHEN legal terminology is necessary, THE System SHALL provide audio explanations with local language analogies
5. THE System SHALL provide information on how to claim or exercise each right mentioned

### Requirement 8: WhatsApp Group Broadcasting

**User Story:** As a community leader, I want to share important scheme information with my village group, so that multiple people can benefit from the information simultaneously.

#### Acceptance Criteria

1. WHEN a user requests to share information, THE System SHALL generate a shareable message format optimized for WhatsApp
2. WHEN sharing to groups, THE Delivery_System SHALL format content with clear headings and bullet points in Regional_Language
3. WHEN scheme deadlines exist, THE System SHALL highlight time-sensitive information prominently
4. WHEN shared content contains links, THE System SHALL include both clickable URLs and SMS-based access codes
5. THE System SHALL track sharing metrics to identify high-impact information for community insights

### Requirement 9: User Profile and Personalization

**User Story:** As a returning user, I want the system to remember my preferences and previous queries, so that I get faster and more relevant responses.

#### Acceptance Criteria

1. WHEN a user interacts for the first time, THE System SHALL create a User_Profile with language preference and basic demographics
2. WHEN a user returns, THE System SHALL retrieve their User_Profile and greet them in their preferred Regional_Language
3. WHEN storing user data, THE System SHALL encrypt personally identifiable information in DynamoDB
4. WHEN a user queries schemes, THE System SHALL prioritize schemes relevant to their demographic profile
5. WHEN a user opts out, THE System SHALL delete their User_Profile within 24 hours
6. THE System SHALL retain interaction history for 90 days for personalization purposes

### Requirement 10: Government API Integration

**User Story:** As a system administrator, I want real-time integration with government APIs, so that users receive accurate and up-to-date information.

#### Acceptance Criteria

1. WHEN querying scheme data, THE API_Gateway SHALL connect to Government_API endpoints (DigiLocker, UMANG) using secure authentication
2. WHEN API response is received, THE System SHALL validate data completeness and accuracy before presenting to users
3. WHEN an API is unavailable, THE System SHALL fall back to cached data and inform the user of potential staleness
4. WHEN API rate limits are reached, THE System SHALL queue requests and notify users of expected wait time
5. THE API_Gateway SHALL log all API interactions for audit and debugging purposes
6. WHEN API data format changes, THE System SHALL handle gracefully and alert administrators

### Requirement 11: Voice Call Handling via IVR

**User Story:** As a user without WhatsApp, I want to call a phone number and speak my query, so that I can access services using basic mobile phones.

#### Acceptance Criteria

1. WHEN a user calls the IVR number, THE Voice_Interface SHALL answer within 3 rings and present language selection menu
2. WHEN language is selected, THE System SHALL present main menu options via audio (schemes, resources, complaints, rights)
3. WHEN a menu option is selected, THE System SHALL route to the appropriate conversation flow
4. WHEN the user is silent for more than 10 seconds, THE System SHALL prompt with helpful guidance
5. WHEN call quality is poor, THE System SHALL offer to switch to SMS mode with the same session context
6. THE Voice_Interface SHALL support DTMF input as alternative to voice commands

### Requirement 12: Scalability and Performance

**User Story:** As a system architect, I want the platform to handle millions of concurrent users, so that we can serve the entire rural Indian population.

#### Acceptance Criteria

1. THE System SHALL support minimum 100,000 concurrent voice sessions without degradation
2. WHEN load exceeds capacity, THE System SHALL queue requests and provide estimated wait time
3. WHEN using AWS services, THE System SHALL leverage auto-scaling to handle traffic spikes
4. THE System SHALL maintain average response time under 5 seconds for 95% of queries
5. WHEN system resources are constrained, THE System SHALL prioritize critical functions (scheme queries, complaints) over non-critical features

### Requirement 13: Data Privacy and Security

**User Story:** As a user sharing personal information, I want my data to be secure and private, so that I can trust the system with sensitive details.

#### Acceptance Criteria

1. WHEN collecting user data, THE System SHALL request only minimum necessary information for service delivery
2. WHEN storing data, THE System SHALL encrypt all personally identifiable information at rest and in transit
3. WHEN accessing Government_API, THE System SHALL use secure authentication tokens that expire after 1 hour
4. WHEN a data breach is detected, THE System SHALL immediately alert administrators and affected users
5. THE System SHALL comply with Indian data protection regulations and government data handling guidelines
6. WHEN users request data deletion, THE System SHALL remove all personal data within 48 hours

### Requirement 14: Analytics and Community Insights

**User Story:** As a policy maker, I want to understand which schemes are most queried and where gaps exist, so that I can improve service delivery.

#### Acceptance Criteria

1. WHEN users interact with the system, THE System SHALL log anonymized query patterns and scheme interest
2. WHEN generating insights, THE System SHALL aggregate data by region, scheme type, and user demographics
3. WHEN trends are identified, THE System SHALL generate weekly reports for administrators
4. WHEN a scheme has low awareness, THE System SHALL flag it for targeted outreach campaigns
5. THE System SHALL track complaint resolution rates and identify systemic issues
6. WHEN sharing analytics, THE System SHALL ensure no individual user can be identified

### Requirement 15: Multi-Channel Message Delivery

**User Story:** As a user, I want to receive information through my preferred channel (WhatsApp, SMS, or voice call), so that I can access services in the most convenient way.

#### Acceptance Criteria

1. WHEN a response is ready, THE Delivery_System SHALL send it via the user's active channel (WhatsApp, IVR, SMS)
2. WHEN WhatsApp delivery fails, THE System SHALL automatically retry via SMS within 30 seconds
3. WHEN sending via SMS, THE System SHALL compress information to fit character limits while maintaining clarity
4. WHEN sending via WhatsApp, THE System SHALL include rich media (images, audio, location pins) where applicable
5. WHEN a user switches channels mid-conversation, THE System SHALL maintain conversation context
6. THE Delivery_System SHALL support message delivery confirmation and retry failed deliveries up to 3 times

### Requirement 16: Dialect and Accent Handling

**User Story:** As a user speaking a regional dialect, I want the system to understand my accent and local expressions, so that I don't have to modify my natural speech.

#### Acceptance Criteria

1. WHEN processing speech, THE Speech_Processor SHALL recognize common rural dialects within each Regional_Language
2. WHEN a dialect is detected, THE System SHALL adapt its speech recognition model to improve accuracy
3. WHEN local expressions are used, THE NLP_Engine SHALL map them to standard intents using dialect dictionaries
4. WHEN the system cannot understand a dialect, THE System SHALL ask clarifying questions in simpler terms
5. THE Speech_Processor SHALL continuously learn from user corrections to improve dialect recognition
6. WHEN responding, THE System SHALL use dialect-appropriate vocabulary and expressions

### Requirement 17: Emergency and Critical Service Access

**User Story:** As a user in an emergency situation, I want to quickly access critical services like ambulances or police, so that I can get help immediately.

#### Acceptance Criteria

1. WHEN a user mentions emergency keywords (ambulance, police, fire, medical emergency), THE System SHALL immediately prioritize the request
2. WHEN an emergency is detected, THE System SHALL provide nearest emergency service contact numbers via audio and SMS
3. WHEN location is available, THE System SHALL share user location with emergency services upon user consent
4. WHEN emergency services are contacted, THE System SHALL provide callback number and reference ID
5. THE System SHALL maintain a database of verified emergency contacts updated monthly
6. WHEN emergency mode is active, THE System SHALL bypass normal conversation flow for immediate assistance

### Requirement 18: Scheme Application Assistance

**User Story:** As a user eligible for a scheme, I want step-by-step guidance to complete the application, so that I can successfully enroll without visiting offices.

#### Acceptance Criteria

1. WHEN a user requests application help, THE System SHALL guide through required documents and information
2. WHEN documents are needed, THE System SHALL explain how to obtain each document in simple terms
3. WHEN the user has DigiLocker access, THE System SHALL integrate to retrieve digital documents
4. WHEN application steps are complex, THE System SHALL break them into small, manageable tasks with checkpoints
5. WHEN an application is submitted, THE System SHALL provide confirmation and tracking information
6. THE System SHALL send reminders for pending applications and upcoming deadlines

### Requirement 19: Feedback and Quality Improvement

**User Story:** As a user, I want to provide feedback on my experience, so that the system can improve and better serve my community.

#### Acceptance Criteria

1. WHEN a conversation ends, THE System SHALL request simple feedback (satisfied/not satisfied) via voice or button press
2. WHEN negative feedback is received, THE System SHALL ask for specific issues via optional voice comment
3. WHEN feedback is collected, THE System SHALL store it with conversation context for analysis
4. WHEN patterns of negative feedback emerge, THE System SHALL alert administrators for investigation
5. THE System SHALL use feedback to retrain NLP models and improve response quality
6. WHEN users provide suggestions, THE System SHALL acknowledge and track for feature prioritization

### Requirement 20: Accessibility for Differently-Abled Users

**User Story:** As a visually impaired user, I want to navigate the system entirely through audio, so that I can access services independently.

#### Acceptance Criteria

1. THE System SHALL provide complete functionality through audio-only interaction without requiring visual elements
2. WHEN presenting options, THE System SHALL clearly enumerate choices with distinct audio cues
3. WHEN confirmation is needed, THE System SHALL accept voice commands (yes/no) or DTMF input
4. WHEN errors occur, THE System SHALL provide clear audio error messages with recovery instructions
5. THE System SHALL support slower speech rates for users who request it
6. WHEN navigation is complex, THE System SHALL offer to repeat instructions or provide additional guidance
