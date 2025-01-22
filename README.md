# Secure-Second-Hand-Marketplace
Buy and Sell Second hand goods securely
= SPEC-001: Second-Hand Marketplace App
:sectnums:
:toc:


== Background

With increasing interest in sustainable living and affordability, the demand for platforms facilitating the buying and selling of second-hand goods is growing. This app aims to provide a secure and user-friendly marketplace where users can trade second-hand products locally and efficiently, fostering a community-driven economy.

The platform will adopt a scalable approach to cater to various regions while maintaining localized features like geolocation. By focusing on security, trust, and seamless transactions, it addresses common pain points such as scammers, unreliable listings, and inefficient communication.

The app will monetize through a commission-based model, ensuring a sustainable business approach without adding a financial burden to users.

== Requirements

The application will target a broad audience while offering localized features to enhance user experience and trust. The requirements are categorized using the MoSCoW framework (Must Have, Should Have, Could Have, Won't Have).

=== Must Have
- User registration with phone and email verification to reduce scammers.
- Profile system with ratings and reviews to ensure reliability.
- Secure chat feature for buyers and sellers to communicate within the app.
- Listing functionality to post second-hand items with images, descriptions, and prices.
- Advanced search and filter options to find items quickly.
- Commission-based transaction system with secure payment integration.
- Geolocation tagging to match buyers and sellers within a specific radius.

=== Should Have
- A reporting system to flag and handle suspicious users or listings.
- Transaction history for both buyers and sellers.
- Local language support to cater to diverse regions.

=== Could Have
- Integration with social media for easy login and promotion of listings.
- Delivery tracking for transactions involving shipment.
- AI-driven fraud detection system to identify scam patterns.

=== Won't Have (initially)
- Auctions or bidding functionality.
- Support for international buyers and sellers.
== Method

This section outlines the technical design of the second-hand marketplace app. The solution will be cloud-based, leveraging Microsoft OneDrive for storing media assets (e.g., product images) and scalable cloud services for backend processing.

=== System Architecture

The system is designed to ensure scalability, reliability, and security. Below is a high-level architecture diagram:

[plantuml]
----
@startuml
actor User
participant "Mobile App" as App
participant "Web App" as Web
participant "Backend API" as API
database "Cloud Database" as DB
participant "OneDrive" as Storage
participant "Payment Gateway" as Payment

User -> App : Interacts with
App -> API : Sends Requests
Web -> API : Sends Requests
API -> DB : Stores/Retrieves Data
API -> Storage : Upload/Fetch Media Files
API -> Payment : Processes Payments
@enduml
----

=== Database Schema

The database will use a relational model with key entities:

1. **Users**
   - `id` (Primary Key)
   - `name`
   - `email`
   - `phone`
   - `password_hash`
   - `rating`
   - `join_date`

2. **Listings**
   - `id` (Primary Key)
   - `user_id` (Foreign Key → Users)
   - `title`
   - `description`
   - `price`
   - `category`
   - `location`
   - `created_at`
   - `updated_at`

3. **Transactions**
   - `id` (Primary Key)
   - `buyer_id` (Foreign Key → Users)
   - `seller_id` (Foreign Key → Users)
   - `listing_id` (Foreign Key → Listings)
   - `status` (e.g., Pending, Completed)
   - `payment_status` (e.g., Paid, Unpaid)
   - `created_at`

4. **Messages**
   - `id` (Primary Key)
   - `sender_id` (Foreign Key → Users)
   - `receiver_id` (Foreign Key → Users)
   - `content`
   - `created_at`

=== Payment Integration Suggestions

For a commission-based model, integrating a payment gateway is crucial. Options include:

1. **Stripe**: Supports commission-based payments and is widely adopted.
2. **PayPal**: Allows secure payments and direct integration for peer-to-peer transactions.
3. **Local Bank Integration**: If users in Overberg prefer bank transfers, consider direct payment API support from major local banks.

**Recommendation**: Use Stripe or PayPal initially for simplicity and scalability. Implementing payment integration at launch is essential to capture commissions on transactions securely.

=== Key Algorithms

1. **Geolocation Matching**
   - Use Haversine formula to calculate distances between users and listings.
   - Filter results based on a configurable radius (e.g., 10km, 50km).

2. **Search and Filters**
   - Full-text search for titles and descriptions.
   - Filters for price range, category, and location.

3. **Fraud Detection**
   - Implement basic rule-based checks for flagged keywords in descriptions.
   - Use machine learning in future iterations to detect patterns of fraudulent activity.

=== Technology Stack

- **Frontend**: React Native (for cross-platform mobile app), React (for web app).
- **Backend**: Node.js with Express.js (RESTful APIs).
- **Database**: PostgreSQL for relational data storage.
- **File Storage**: Microsoft OneDrive for image uploads and retrieval.
- **Payment Gateway**: Stripe or PayPal.
- **Hosting**: Azure App Services for backend and database hosting.
== Method

This section outlines the technical design of the second-hand marketplace app. The solution will be cloud-based, leveraging Microsoft OneDrive for storing media assets (e.g., product images) and scalable cloud services for backend processing.

=== System Architecture

The system is designed to ensure scalability, reliability, and security. Below is a high-level architecture diagram:

[plantuml]
----
@startuml
actor User
participant "Mobile App" as App
participant "Web App" as Web
participant "Backend API" as API
database "Cloud Database" as DB
participant "OneDrive" as Storage
participant "Stripe Payment Gateway" as Stripe

User -> App : Interacts with
App -> API : Sends Requests
Web -> API : Sends Requests
API -> DB : Stores/Retrieves Data
API -> Storage : Upload/Fetch Media Files
API -> Stripe : Processes Payments
@enduml
----

=== Database Schema

The database will use a relational model with key entities:

1. **Users**
   - `id` (Primary Key)
   - `name`
   - `email`
   - `phone`
   - `password_hash`
   - `rating`
   - `join_date`

2. **Listings**
   - `id` (Primary Key)
   - `user_id` (Foreign Key → Users)
   - `title`
   - `description`
   - `price`
   - `category`
   - `location`
   - `created_at`
   - `updated_at`

3. **Transactions**
   - `id` (Primary Key)
   - `buyer_id` (Foreign Key → Users)
   - `seller_id` (Foreign Key → Users)
   - `listing_id` (Foreign Key → Listings)
   - `status` (e.g., Pending, Completed)
   - `payment_status` (e.g., Paid, Unpaid)
   - `created_at`

4. **Messages**
   - `id` (Primary Key)
   - `sender_id` (Foreign Key → Users)
   - `receiver_id` (Foreign Key → Users)
   - `content`
   - `created_at`

=== Payment Integration with Stripe

Stripe will be used to process payments securely and handle the commission model. Key features include:

- **Payment Flow**:
  1. Buyers pay for the item through the app.
  2. Stripe collects payment and deducts the commission fee.
  3. The remaining amount is transferred to the seller's account.

- **Stripe Features Utilized**:
  - **Connect Accounts**: Allows sellers to link their Stripe accounts.
  - **Split Payments**: Automates commission deduction.
  - **Fraud Prevention**: Built-in mechanisms to identify and prevent fraudulent transactions.

=== Key Algorithms

1. **Geolocation Matching**
   - Use Haversine formula to calculate distances between users and listings.
   - Filter results based on a configurable radius (e.g., 10km, 50km).

2. **Search and Filters**
   - Full-text search for titles and descriptions.
   - Filters for price range, category, and location.

3. **Fraud Detection**
   - Implement basic rule-based checks for flagged keywords in descriptions.
   - Use machine learning in future iterations to detect patterns of fraudulent activity.

=== Technology Stack

- **Frontend**: React Native (for cross-platform mobile app), React (for web app).
- **Backend**: Node.js with Express.js (RESTful APIs).
- **Database**: PostgreSQL for relational data storage.
- **File Storage**: Microsoft OneDrive for image uploads and retrieval.
- **Payment Gateway**: Stripe.
- **Hosting**: Azure App Services for backend and database hosting.
== Implementation

This section provides a step-by-step plan to implement the second-hand marketplace app, ensuring a clear roadmap for development.

=== Phase 1: Planning and Setup

1. **Requirement Finalization**:
   - Confirm all app features and finalize UI/UX designs.
   - Create a project plan with timelines and milestones.

2. **Development Environment Setup**:
   - Set up repositories (e.g., GitHub or GitLab) for code management.
   - Configure development tools and CI/CD pipelines.
   - Establish cloud services (e.g., Azure App Services, Microsoft OneDrive API, Stripe).

=== Phase 2: Core Development

1. **Backend Development**:
   - Build RESTful APIs using Node.js and Express.js.
   - Set up PostgreSQL database with the defined schema.
   - Integrate Microsoft OneDrive for image storage.
   - Implement Stripe payment gateway for secure transactions.

2. **Frontend Development**:
   - Develop a cross-platform mobile app using React Native.
   - Create a responsive web app using React.
   - Design user-friendly interfaces with focus on accessibility and simplicity.

3. **Key Features**:
   - User authentication (phone/email verification).
   - Listing creation and management.
   - Geolocation-based search and filters.
   - Messaging system for buyer-seller communication.
   - Transaction processing and commission handling.

=== Phase 3: Testing

1. **Unit Testing**:
   - Test individual components (e.g., user authentication, payment processing).

2. **Integration Testing**:
   - Verify end-to-end functionality (e.g., from listing creation to payment completion).

3. **User Acceptance Testing (UAT)**:
   - Conduct testing with a small group of target users to ensure usability and reliability.

=== Phase 4: Deployment

1. **Staging Environment**:
   - Deploy the app on a staging environment to test in a live-like scenario.

2. **Production Deployment**:
   - Deploy the backend and database on Azure App Services.
   - Publish the mobile app on Google Play Store and Apple App Store.
   - Launch the web app with a scalable hosting solution.

=== Phase 5: Maintenance and Iteration

1. **Monitoring and Analytics**:
   - Use tools like Google Analytics and Azure Monitor to track performance and usage.
   - Monitor for fraud or security breaches.

2. **Iterative Enhancements**:
   - Implement user feedback to improve the app.
   - Add features like delivery tracking or AI-based fraud detection as needed.
Listings Table:
- Add `is_auction` (boolean): Specifies if the listing is an auction.
- Add `starting_price` (decimal): Minimum price to start the bidding.
- Add `buy_it_now_price` (decimal, optional): Price to immediately purchase the item.
- Add `auction_end_time` (datetime): Time when the auction ends.

Bids Table:
- `id` (Primary Key)
- `listing_id` (Foreign Key → Listings)
- `user_id` (Foreign Key → Users)
- `bid_amount` (decimal)
- `bid_time` (datetime)

== Method

This section outlines the technical design of the second-hand marketplace app, now including auction functionality. The solution will be cloud-based, leveraging Microsoft OneDrive for media storage, Stripe for payment processing, and scalable backend services.

=== System Architecture

The system now includes real-time bid management and auction-specific workflows. Below is the updated high-level architecture diagram:

[plantuml]
----
@startuml
actor User
participant "Mobile App" as App
participant "Web App" as Web
participant "Backend API" as API
database "Cloud Database" as DB
participant "OneDrive" as Storage
participant "Stripe Payment Gateway" as Stripe

User -> App : Interacts with
App -> API : Sends Requests
Web -> API : Sends Requests
API -> DB : Stores/Retrieves Data
API -> Storage : Upload/Fetch Media Files
API -> Stripe : Processes Payments
API -> User : Sends Notifications (e.g., Bid Updates, Auction End)
@enduml
----

=== Database Schema (Updated)

1. **Users**
   - Unchanged.

2. **Listings**
   - `id` (Primary Key)
   - `user_id` (Foreign Key → Users)
   - `title`
   - `description`
   - `price`
   - `is_auction` (boolean): Indicates if the listing is an auction.
   - `starting_price` (decimal): Minimum price to start bidding.
   - `buy_it_now_price` (decimal, optional): Price to bypass the auction.
   - `auction_end_time` (datetime): Time when the auction ends.
   - Other fields remain unchanged.

3. **Bids**
   - `id` (Primary Key)
   - `listing_id` (Foreign Key → Listings)
   - `user_id` (Foreign Key → Users)
   - `bid_amount` (decimal)
   - `bid_time` (datetime)

4. **Transactions**
   - Unchanged.

5. **Messages**
   - Unchanged.

=== Key Features

1. **Auction Management**:
   - Users can create auction listings with a starting price, optional "Buy-It-Now" price, and end time.
   - Buyers can place bids on auction listings, with real-time updates.
   - The highest bidder at auction end wins the item.

2. **Real-Time Bid Updates**:
   - WebSocket or server-sent events (SSE) for live bid updates.

3. **Buy-It-Now Option**:
   - Buyers can bypass the auction by paying a pre-set price.
   - Auction ends immediately after a "Buy-It-Now" transaction.

4. **Winner Notification**:
   - Notify the winning bidder and seller via in-app notifications and email.

5. **Timer Logic**:
   - Background job queues (e.g., Bull with Redis) manage auction timers and handle bid cutoffs.

=== Key Algorithms

1. **Bid Validation**:
   - Ensure the new bid is higher than the current highest bid.
   - Reject bids after the auction end time.

2. **Winner Determination**:
   - At auction end, finalize the highest bid as the winner.
   - Notify both parties and initiate the payment process.

3. **Buy-It-Now Handling**:
   - Mark the item as sold immediately.
   - Cancel the auction and notify participants.

=== Technology Stack

- **Frontend**: React Native (mobile), React (web) with WebSocket support for live updates.
- **Backend**: Node.js with Express.js for APIs, Bull with Redis for auction timers.
- **Database**: PostgreSQL with schema updates for auctions and bids.
- **File Storage**: Microsoft OneDrive for media.
- **Payment Gateway**: Stripe (includes escrow for winning bids).
- **Hosting**: Azure App Services for backend and database.
== Implementation

This section provides a step-by-step roadmap to build the second-hand marketplace app, now including auction functionality.

=== Phase 1: Planning and Setup

1. **Requirement Finalization**:
   - Finalize all app features, including auction workflows.
   - Design UI/UX for auction-specific screens (e.g., bidding, "Buy-It-Now").

2. **Development Environment Setup**:
   - Set up repositories (e.g., GitHub or GitLab) for code versioning.
   - Configure development tools, CI/CD pipelines, and cloud services (Azure, OneDrive, Stripe).

=== Phase 2: Core Development

1. **Backend Development**:
   - Build RESTful APIs using Node.js and Express.js.
   - Set up PostgreSQL database with the auction-enhanced schema.
   - Implement Stripe integration for payment processing (including escrow).
   - Create background jobs using Bull and Redis for auction timer management.
   - Add WebSocket or SSE support for real-time bid updates.

2. **Frontend Development**:
   - Mobile App (React Native):
     - Auction-specific screens (e.g., bid placement, live updates).
     - Real-time bid notifications.
   - Web App (React):
     - Responsive auction interfaces.
     - Real-time bid display using WebSocket or SSE.
   - Ensure seamless integration of auction features with standard functionalities (e.g., search, messaging).

=== Phase 3: Testing

1. **Unit Testing**:
   - Test API endpoints (e.g., bid placement, timer workflows).
   - Validate database interactions for bids and auctions.

2. **Integration Testing**:
   - Simulate user flows: auction creation, bidding, and "Buy-It-Now" completion.
   - Ensure real-time bid updates function reliably under load.

3. **User Acceptance Testing (UAT)**:
   - Conduct testing with a small group of target users to validate the auction experience.
   - Gather feedback on usability and performance.

=== Phase 4: Deployment

1. **Staging Environment**:
   - Deploy the app on a staging environment for final testing.

2. **Production Deployment**:
   - Deploy the backend, database, and Redis on Azure App Services.
   - Publish the mobile app to Google Play Store and Apple App Store.
   - Launch the web app with a scalable hosting solution.

=== Phase 5: Maintenance and Iteration

1. **Monitoring and Analytics**:
   - Track auction-specific metrics (e.g., average bids, "Buy-It-Now" usage).
   - Monitor for unusual activity (e.g., bid spamming) to identify potential fraud.

2. **Iterative Enhancements**:
   - Implement user feedback to refine the auction experience.
   - Add advanced fraud detection and AI-powered recommendations in future updates.
== Milestones

To ensure a structured approach to the development of the second-hand marketplace app, the following milestones have been identified:

=== Phase 1: Planning and Setup (Week 1–2)
- Finalize app requirements, including auction functionality.
- Complete UI/UX designs for all key workflows.
- Set up project repositories, development tools, and cloud services.

=== Phase 2: Core Development (Week 3–10)
- Backend:
  - Build APIs for user registration, product listings, and messaging. (Week 3–5)
  - Implement auction logic, including bid placement and timer workflows. (Week 6–7)
  - Integrate Stripe for payments and escrow management. (Week 8)
- Frontend:
  - Develop core screens (e.g., login, product listings, search). (Week 3–6)
  - Implement auction screens with real-time bid updates. (Week 7–9)
  - Integrate payment workflows with Stripe. (Week 9–10)

=== Phase 3: Testing (Week 11–12)
- Perform unit testing on all components.
- Conduct integration testing for end-to-end auction flows.
- Perform user acceptance testing (UAT) with a small test group.

=== Phase 4: Deployment (Week 13)
- Deploy the backend, database, and Redis to Azure App Services.
- Publish the mobile app on Google Play Store and Apple App Store.
- Launch the web app with full functionality.

=== Phase 5: Maintenance and Iteration (Ongoing)
- Monitor app performance and analytics.
- Address bugs and user feedback.
- Plan enhancements like AI fraud detection and delivery tracking for future updates.
== Gathering Results

Once the app is live, its performance and user satisfaction will be evaluated to determine if it meets the outlined requirements and goals.

=== Evaluation Metrics

1. **User Engagement**:
   - Number of active users daily and monthly.
   - Growth in new user registrations.
   - Retention rates over time.

2. **Marketplace Activity**:
   - Number of product listings created.
   - Frequency of transactions, including auctions.
   - Average bid activity per auction.

3. **Revenue Generation**:
   - Total commission earned via Stripe payments.
   - Usage of the "Buy-It-Now" feature.

4. **User Trust and Satisfaction**:
   - Average user rating and feedback.
   - Number of disputes or flagged listings.
   - Reduction in scam-related complaints.

5. **System Performance**:
   - API response times and app stability.
   - Real-time auction updates reliability.
   - Scalability under peak loads.

=== Post-Launch Actions

1. **User Feedback Collection**:
   - Use surveys and in-app feedback forms to collect user suggestions.
   - Monitor reviews on app stores and social media.

2. **Continuous Monitoring**:
   - Track real-time analytics using tools like Google Analytics and Azure Monitor.
   - Identify and resolve bottlenecks or bugs promptly.

3. **Feature Enhancements**:
   - Based on feedback and metrics, prioritize features like AI-based fraud detection and delivery tracking for future updates.

=== Success Criteria

The app will be considered successful if:
- It achieves a strong initial user base with sustained growth.
- Users actively create listings, participate in auctions, and complete transactions.
- Revenue from commissions meets the expected benchmarks.
- Users report high levels of satisfaction and trust in the platform.
= SPEC-001: Second-Hand Marketplace App
:sectnums:
:toc:


== Background

Overberg is a region with a thriving community interested in sustainable living and affordability, making it an ideal target for a second-hand product marketplace. Many residents prefer local buying and selling to minimize transportation costs and environmental impact.

This app aims to provide a secure and user-friendly marketplace where users can trade second-hand products locally and efficiently, fostering a community-driven economy. The platform will adopt a scalable approach to cater to various regions while maintaining localized features like geolocation. By focusing on security, trust, and seamless transactions, it addresses common pain points such as scammers, unreliable listings, and inefficient communication.

The app will monetize through a commission-based model, ensuring a sustainable business approach without adding a financial burden to users.


== Requirements

The application will target a broad audience while offering localized features to enhance user experience and trust. The requirements are categorized using the MoSCoW framework (Must Have, Should Have, Could Have, Won't Have).

=== Must Have
- User registration with phone and email verification to reduce scammers.
- Profile system with ratings and reviews to ensure reliability.
- Secure chat feature for buyers and sellers to communicate within the app.
- Listing functionality to post second-hand items with images, descriptions, and prices.
- Advanced search and filter options to find items quickly.
- Commission-based transaction system with secure payment integration.
- Geolocation tagging to match buyers and sellers within a specific radius.
- Auction functionality with "Buy-It-Now" options.

=== Should Have
- A reporting system to flag and handle suspicious users or listings.
- Transaction history for both buyers and sellers.
- Local language support to cater to diverse regions.

=== Could Have
- Integration with social media for easy login and promotion of listings.
- Delivery tracking for transactions involving shipment.
- AI-driven fraud detection system to identify scam patterns.

=== Won't Have (initially)
- Support for international buyers and sellers.


== Method

This section outlines the technical design of the second-hand marketplace app, now including auction functionality. The solution will be cloud-based, leveraging Microsoft OneDrive for media storage, Stripe for payment processing, and scalable backend services.

=== System Architecture
(Refer to PlantUML diagram in prior sections)

=== Database Schema
(Details included in prior sections)

=== Key Features and Algorithms
(Real-time bidding, fraud detection, etc.)

=== Technology Stack
- **Frontend**: React Native and React.
- **Backend**: Node.js and Express.js.
- **Database**: PostgreSQL.
- **File Storage**: Microsoft OneDrive.
- **Payment Gateway**: Stripe.
- **Hosting**: Azure App Services.

== Implementation

(Refer to structured steps and timelines in prior sections.)

== Milestones

(Refer to structured phases in prior sections.)

== Gathering Results

(Evaluation metrics, post-launch actions, and success criteria in prior sections.)
