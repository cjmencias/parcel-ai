## Parcel.AI Domain

**Parcel.ai** is an on-demand parcel delivery platform that connects customers who need items delivered with nearby drivers. The platform provides real-time tracking, and a seamless experience across mobile and web interfaces.

### Key Features
- **Social Sign-In:** Quick OAuth authentication with Google
- **Instant Booking:** Customers can book deliveries with fare estimates
- **Simple Dispatch:** Assigns jobs to specific or nearest available driver
- **Live Tracking:** Real-time location updates
- **Multi-role Platform:** Single mobile app serving customers and drivers, web interface for operators
- **Rating System:** Post-delivery driver rating for quality assurance
- **Booking Management:** Cancel bookings with reason tracking
- **Operator Dashboard:** Web-based driver and booking management

---

## MVP User Stories

### US-01: Social Sign-In

**As a** new user  
**I want to** sign up or log in with Google
**So that** I can start booking without creating a password

**Acceptance Criteria:**
- Sign-in with Google button is visible on the welcome screen
- Upon successful OAuth, the user lands on the Home screen already authenticated
- If OAuth fails, an error message explains the issue

### US-03: Create Booking with Fare Estimate

**As a** user  
**I want to** input parcel & route details and see the fare before I confirm  
**So that** I know the cost upfront

**Acceptance Criteria:**
- Mandatory fields: pickup, destination, parcel size/weight
- System displays fare ≤ 3 seconds after fields completed
- "Book" button is disabled until all required fields are valid

### US-04: Cancel Booking (with reason)

**As a** customer  
**I want to** cancel a booking while it's pending or assigned and provide an explanation  
**So that** I'm not charged and drivers are informed

**Acceptance Criteria:**
- Cancel booking action is available while the job status is "pending" or "assigned"
- Tapping Cancel shows a short list of reasons plus an optional text box for extra details
- A confirmation pop-up asks the customer to confirm the cancellation
- After confirmation:
  - The job is marked Cancelled and the customer's card is not charged
  - The assigned driver (if any) is informed that the job was cancelled
  - The booking now appears in the customer's history with the label Cancelled and the chosen reason visible

### US-08: Mobile-Optimized Field Operations

**As a** delivery driver  
**I want to** access and update job information on my mobile device with touch-friendly interactions  
**So that** I can efficiently manage deliveries while on the road without desktop dependency

**Acceptance Criteria:**
- Responsive design optimized for mobile screens
- Touch-friendly buttons and forms
- Fast loading performance for field use
- Be able to update job status such as when it's started, and completed

### US-09: Receive & Accept Job

**As a** driver  
**I want to** receive nearby job offers with payout info and accept the one I want  
**So that** I can earn money

**Acceptance Criteria:**
- Push notification arrives within 2s of dispatch
- Job card shows pickup, drop-off, payout
- System supports both operator manual assignment to specific driver or auto-dispatch to nearest available drivers
- For auto-dispatched jobs: First-acceptor-wins model where the driver can key in their ETA
- For manually assigned jobs: Driver receives direct assignment notification

### US-11: Real-Time Tracking & Live Map with ETA

**As a** user  
**I want to** see the driver's live location and ETA with real-time status updates  
**So that** I know when to expect pickup and delivery

**Acceptance Criteria:**
- Driver's ETA is shown
- Map updates every 15 seconds with driver's live location

### US-16: Rating Flow

**As a** user  
**I want to** rate the driver after delivery  
**So that** service quality stays high

**Acceptance Criteria:**
- 1-5 star modal appears after job completion
- Rating is optional but can't be edited after submission

### US-20: Operator Management Interface

**As an** operator  
**I want to** manage drivers and bookings through a web interface  
**So that** I can oversee daily operations

**Acceptance Criteria:**
- Driver management: view list, suspend/reactivate drivers
- Booking management: search by booking ID, customer, driver
- Driver status changes reflect immediately in mobile app

---

## Business Rules & Logic

### Pricing Calculation
- **Base Calculation:** Base fare + (distance × per-km rate) = Driver Base Amount
- **Commission:** Platform commission = Driver Base Amount × commission %
- **Tax:** GST/VAT = (Driver Base Amount + Commission) × tax %
- **Customer Fare:** Driver Base Amount + Commission + Tax
- **Driver Receives:** Driver Base Amount (commission and tax are deducted from customer payment)
- **Platform Keeps:** Commission + Tax
- **Fare Display:** System must display customer fare ≤ 3 seconds after route details completed
- **Currency:** System uses single currency per deployment, typically configured during initial setup and rarely changed
- **Rate Configuration:** Only operators can modify commission %, tax %, base fare, per-km rates, and system currency
- **Rate Application:** Changes apply only to new bookings created after the update

**Example Calculation (USD):**
- **Base fare:** $5.00
- **Distance:** 3 km × $1.50/km = $4.50
- **Driver Base Amount:** $5.00 + $4.50 = $9.50
- **Commission (20%):** $9.50 × 0.20 = $1.90
- **Tax (10%):** $11.40 × 0.10 = $1.14
- **Subtotal:** $9.50 + $1.90 = $11.40
- **Customer Pays:** $11.40 + $1.14 = $12.54
- **Driver Receives:** $9.50
- **Platform Keeps:** $1.90 + $1.14 = $3.04

### Driver Management
- **Registration Process:** Operators register drivers through the operator portal with required documentation
- **Required Fields:** Name, ID photo, vehicle plate, bank info
- **Status Management:** pending → active ⟷ suspended (operators can directly reactivate suspended drivers)
- **Eligibility:** Only active drivers with complete signup receive job offers

### Job Dispatch Logic
- **Manual Assignment:** System operators can directly assign jobs to specific drivers through operator portal
- **Auto-Dispatch Mode:** Proximity-based dispatch where nearest eligible drivers receive jobs first based on current location proximity to pickup point
- **First-Acceptor-Wins:** First driver to tap "Accept" gets the job (applies to auto-dispatched jobs only)
- **Direct Assignment:** Job assigned to specific driver (no acceptance required, driver notified of assignment)
- **Broadcast Assignment:** Job offered to multiple nearby drivers based on proximity with first-acceptor-wins logic
- **Timeout Handling:** Auto-dispatched jobs withdrawn if unclaimed after timeout period (default: 5 minutes, operator configurable)
- **Location Tracking:** System uses real-time driver locations to calculate proximity for optimal dispatch
- **Notification Speed:** Push notifications must arrive within 2 seconds of dispatch

### Booking Lifecycle
- **Status Flow:** pending → assigned → picked_up → in_transit → delivered → completed
- **Cancellation Window:** Available while job status is "pending" or "assigned"


### Real-time Tracking
- **Initial ETA:** Calculated when driver accepts job based on current location and route
- **Update Method:** Manual refresh to get latest driver location and ETA

### Rating System
- **Rating Window:** 1-5 star modal appears after job completion
- **Optional Rating:** Rating submission is optional and does not affect driver eligibility
- **Finality:** Ratings cannot be edited after submission
- **Driver Impact:** Ratings provide light preference in dispatch when drivers are at similar distances (small effect to maintain fairness for all drivers)

### Operator Controls
- **Search Capabilities:** Search by booking ID, customer name, or driver name
- **Driver Management:** Register drivers through operator portal, real-time status toggle between active/suspended (no need to return to pending) reflects immediately in app
- **Rate Configuration:** Full control over pricing parameters (base fare, per-km rates, commission %, tax %) and system currency with validation (currency changes not recommended post-deployment)
- **Job Dispatch:** Manual assignment to specific drivers or auto-dispatch to nearest available drivers based on current location proximity

### Booking History
- **Booking History:** Paginated list of past bookings with detail view access

---

## Technology Stack

### API Backend
- **Ruby on Rails** with **GraphQL** for flexible, efficient API queries
- **SQLite3** for local development and testing (lightweight, file-based database)
- **ActionCable** for real-time communication: live tracking, push notifications, and job dispatch updates

### Web Frontend
- **Vite React** application for fast development and build times
- **Tailwind CSS** for utility-first styling and responsive design
- **Apollo Client** for GraphQL integration

### Mobile Application
- **Expo React Native** for cross-platform mobile development (iOS, Android)
- **Tailwind CSS** via NativeWind for consistent styling with web
- **Apollo Client** for GraphQL integration
- Focus on mobile-first experience for the hackathon event

## Technical Architecture

### Mobile App Responsibilities (Expo React Native)
- **User Interface:** Role-based views for Customer and Driver
- **State Management:** Apollo Client cache, local app state, user sessions
- **Client-side Logic:** Form validation, navigation, UI interactions
- **Device Integration:** GPS for location tracking, push notifications
- **GraphQL Integration:** Apollo Client for efficient data fetching
- **Offline Capabilities:** Apollo cache for offline data access

### Web Dashboard Responsibilities (Vite React)
- **Operator Interface:** Driver and booking management
- **State Management:** Apollo Client cache, React state
- **GraphQL Integration:** Apollo Client for data operations
- **Responsive Design:** Tailwind CSS for mobile-friendly operator access

### Backend Responsibilities (Ruby on Rails + GraphQL)
- **GraphQL API:** Single endpoint for all client operations
- **Authentication:** JWT-based auth with Google OAuth integration
- **Business Logic:** Booking lifecycle, simple dispatch algorithm
- **Database Operations:** ActiveRecord with SQLite3
- **Background Jobs:** Sidekiq for async processing

### Third-party Integrations (Development Mode)
- **Authentication:** Google Sign-In (development credentials)
- **Maps & Geolocation:** Google Maps Platform (development API key)
- **Push Notifications:** Expo Push Notifications (development tokens)

### Database Schema Overview
- **Core Entities:** Users, Drivers, Bookings, Ratings
- **Geospatial Data:** Driver locations, pickup/dropoff coordinates (stored as latitude/longitude pairs)
- **Status Management:** Booking states, driver states
- **Simple Tracking:** Basic location history for ETA calculations

---

## Application Architecture Decision

### Single App with Role-Based Views
**Decision:** Build one React Native app that serves all user types (Customer, Driver, Operator) with different interfaces based on user role.

**Rationale:**
- **Shared Codebase:** Faster development and easier maintenance
- **Common Components:** Maps, payments, notifications, and support features reused across roles
- **Single Deployment:** One app to build, test, and deploy
- **Consistency:** Easier to maintain UI/UX consistency across user types
- **Cost Efficiency:** Lower development and maintenance costs
- **User Flexibility:** Users can potentially have multiple roles (customer + driver)

**App Structure:**
```
├── Mobile App (Expo React Native)
│   ├── Authentication (Google OAuth)
│   ├── Customer Interface
│   │   ├── Booking creation with fare estimate
│   │   ├── Live tracking and ETA
│   │   ├── Cancel booking with reason
│   │   └── Rating flow
│   └── Driver Interface
│       ├── Job acceptance and management
│       ├── Real-time location updates
│       └── Job status updates
└── Web Dashboard (Vite React)
    └── Operator Interface
        ├── Driver management (create/suspend)
        └── Booking search and management
```

---

## Design System & UI/UX

### Color Scheme
- **Primary:** Blue gradients (#0ea5e9 to #0369a1) - Main brand colors for primary actions and branding
- **Secondary:** Gray tones for neutral actions and secondary elements
- **Success:** Green gradient for completed states, successful operations, and positive feedback
- **Warning:** Yellow gradient for pending states, cautions, and intermediate status
- **Danger:** Red gradient for cancelled/error states, destructive actions, and alerts
- **Accent:** Purple gradients (#d946ef to #a21caf) - Highlight colors for special features and emphasis

### Typography
- **Font Family:** Inter from Google Fonts - Modern, clean, and highly readable
- **Font Weights:** 300 (Light), 400 (Regular), 500 (Medium), 600 (Semi-bold), 700 (Bold), 800 (Extra-bold)
- **Typography Effects:** Modern typography with gradient text effects for headings and emphasis
- **Hierarchy:** Clear typographic hierarchy supporting role-based interfaces and information density

### Design Features
- **Glassmorphism:** Beautiful glass effects with backdrop blur for modern, layered interface elements
- **Gradient Backgrounds:** Stunning color gradients throughout the interface for visual depth and brand consistency
- **Smooth Animations:** 
  - Fade-in effects for content loading
  - Slide-up animations for modals and sheets
  - Bounce effects for interactive feedback
- **Interactive Elements:** 
  - Hover effects for web interface
  - Scale animations on touch for mobile
  - Lift transitions for elevated components
- **Progress Visualization:** Animated progress bars showing job completion status and real-time updates
- **Loading States:** Beautiful loading spinners and skeleton screens for seamless user experience during data loading

### User Experience Principles
- **Visual Hierarchy:** Clear information architecture using gradients, typography, and spacing
- **Feedback Systems:** Immediate visual feedback for all user interactions through animations and color changes
- **Consistency:** Unified design language across all three user roles (Customer, Driver, Operator)
- **Accessibility:** High contrast ratios, readable font sizes, and touch-friendly interactive elements
- **Performance:** Optimized animations and effects that don't compromise app performance

### Role-Specific Design Considerations
- **Customer Interface:** 
  - Clean, consumer-friendly design with emphasis on booking flow and tracking
  - Prominent call-to-action buttons with gradient styling
  - Real-time status updates with animated progress indicators
- **Driver Interface:** 
  - Functional, efficiency-focused design for quick job management
  - Large, touch-friendly buttons for in-vehicle use
  - Clear visual hierarchy for job information and navigation
- **Operator Interface:** 
  - Data-dense design optimized for desktop/tablet use
  - Advanced filtering and search capabilities with clear visual feedback
  - Dashboard-style layout with comprehensive information display

### Component Standards
- **Buttons:** Gradient backgrounds with smooth hover/press animations
- **Cards:** Glassmorphism effects with subtle shadows and backdrop blur
- **Forms:** Clean input fields with gradient focus states and validation feedback
- **Navigation:** Smooth transitions between screens with consistent iconography
- **Status Indicators:** Color-coded with gradient styling and animated state changes
- **Maps:** Integrated with glassmorphism overlays and gradient markers

---

## Data Models & Relationships

### Core Entities

#### Users
```json
{
  "id": "uuid",
  "email": "string",
  "name": "string",
  "role": "customer | driver | operator",
  "auth_provider": "google",
  "auth_provider_id": "string",
  "profile_image_url": "string",
  "created_at": "timestamp",
  "is_active": "boolean"
}
```

#### Drivers (extends Users)
```json
{
  "user_id": "uuid (FK to users.id)",
  "status": "pending | active | suspended",
  "vehicle_plate": "string",
  "id_photo_url": "string",
  "bank_info": "text (JSON string)",
  "current_lat": "decimal(10,8)",
  "current_lng": "decimal(11,8)",
  "rating_average": "decimal(2,1)",
  "total_jobs": "integer",
  "created_at": "timestamp",
  "updated_at": "timestamp"
}
```

#### Bookings
```json
{
  "id": "uuid",
  "customer_id": "uuid (FK to users.id)",
  "driver_id": "uuid (FK to users.id, nullable)",
  "pickup_lat": "decimal(10,8)",
  "pickup_lng": "decimal(11,8)",
  "pickup_address": "string",
  "dropoff_lat": "decimal(10,8)",
  "dropoff_lng": "decimal(11,8)",
  "dropoff_address": "string",
  "parcel_size": "small | medium | large",
  "status": "pending | assigned | picked_up | in_transit | delivered | completed | cancelled",
  "assignment_type": "auto_dispatch | manual_assignment",
  "fare_amount": "decimal(10,2)",
  "driver_amount": "decimal(10,2)",
  "commission_amount": "decimal(10,2)",
  "tax_amount": "decimal(10,2)",
  "estimated_distance_km": "decimal",
  "estimated_duration_minutes": "integer",
  "pickup_eta": "timestamp (nullable)",
  "delivery_eta": "timestamp (nullable)",
  "cancellation_reason": "string (nullable)",
  "created_at": "timestamp",
  "assigned_at": "timestamp (nullable)",
  "picked_up_at": "timestamp (nullable)",
  "delivered_at": "timestamp (nullable)",
  "completed_at": "timestamp (nullable)"
}
```

#### Pricing Configuration
```json
{
  "id": "uuid",
  "base_fare": "decimal(10,2)",
  "per_km_rate": "decimal(10,2)",
  "commission_percentage": "decimal(5,2)",
  "tax_percentage": "decimal(5,2)",
  "currency": "string",
  "is_active": "boolean",
  "created_at": "timestamp",
  "created_by": "uuid (FK to users.id)"
}
```

#### Driver Locations
```json
{
  "id": "uuid",
  "driver_id": "uuid (FK to users.id)",
  "booking_id": "uuid (FK to bookings.id, nullable)",
  "lat": "decimal(10,8)",
  "lng": "decimal(11,8)",
  "timestamp": "timestamp"
}
```

#### Ratings
```json
{
  "id": "uuid",
  "booking_id": "uuid (FK to bookings.id)",
  "customer_id": "uuid (FK to users.id)",
  "driver_id": "uuid (FK to users.id)",
  "rating": "integer (1-5)",
  "comment": "text (nullable)",
  "created_at": "timestamp"
}
```

### Status Enums & Transitions

#### Booking Status Flow
```
pending → assigned → picked_up → in_transit → delivered → completed
   ↓         ↓
cancelled ←──┘
```

#### Driver Status Flow
```
pending → active ⟷ suspended
(operators register drivers as pending, then activate)
(operators can toggle between active and suspended)
```

#### Assignment Types
```
auto_dispatch: Broadcast to nearby drivers (first-acceptor-wins)
manual_assignment: Direct assignment to specific driver
```

### Key Relationships

- **Users** (1) → (many) **Bookings** (as customer)
- **Users** (1) → (many) **Bookings** (as driver)
- **Users** (1) → (1) **Drivers** (for driver role)
- **Bookings** (1) → (many) **Driver Locations** (tracking history)
- **Bookings** (1) → (1) **Ratings** (optional, post-completion)
- **Drivers** (1) → (many) **Driver Locations** (location history)
- **Pricing Configuration** (1) → (many) **Bookings** (fare calculation)
- **Users** (1) → (many) **Pricing Configuration** (operators create rates)

### Indexes & Performance Considerations

#### Critical Indexes
- `bookings.status` - For filtering active bookings
- `bookings.customer_id, bookings.created_at` - Customer history
- `bookings.driver_id, bookings.created_at` - Driver history
- `bookings.assignment_type` - Dispatch method filtering
- `driver_locations.driver_id, driver_locations.timestamp` - Real-time tracking
- `drivers.status` - Active driver queries
- `drivers.current_location` - Spatial index for nearby driver queries
- `pricing_configuration.is_active` - Current pricing lookup
- `ratings.driver_id, ratings.created_at` - Driver rating calculations

#### Geospatial Considerations
- Location data stored as separate latitude/longitude columns for SQLite compatibility
- Distance calculations using Haversine formula for driver proximity
- Simple coordinate-based queries for location filtering

---

## GraphQL API Specifications

### Authentication
- **Endpoint:** `https://api.parcelai.com/graphql`
- **Authentication:** Bearer JWT tokens in Authorization header

### GraphQL Schema

#### Types
```graphql
type User {
  id: ID!
  email: String!
  name: String!
  role: UserRole!
  authProvider: AuthProvider!
  profileImageUrl: String
  createdAt: DateTime!
  isActive: Boolean!
}

type Driver {
  id: ID!
  user: User!
  status: DriverStatus!
  vehiclePlate: String!
  idPhotoUrl: String
  bankInfo: JSON
  currentLocation: Location
  ratingAverage: Float
  totalJobs: Int!
  createdAt: DateTime!
  updatedAt: DateTime!
}

type Booking {
  id: ID!
  customer: User!
  driver: Driver
  pickupLocation: Location!
  pickupAddress: String!
  dropoffLocation: Location!
  dropoffAddress: String!
  parcelSize: ParcelSize!
  status: BookingStatus!
  assignmentType: AssignmentType!
  fareAmount: Float!
  driverAmount: Float!
  commissionAmount: Float!
  taxAmount: Float!
  estimatedDistanceKm: Float!
  estimatedDurationMinutes: Int!
  pickupEta: DateTime
  deliveryEta: DateTime
  cancellationReason: String
  createdAt: DateTime!
  assignedAt: DateTime
  pickedUpAt: DateTime
  deliveredAt: DateTime
  completedAt: DateTime
}

type FareEstimate {
  fareAmount: Float!
  driverAmount: Float!
  commissionAmount: Float!
  taxAmount: Float!
  estimatedDistanceKm: Float!
  estimatedDurationMinutes: Int!
  currency: String!
}

type PricingConfiguration {
  id: ID!
  baseFare: Float!
  perKmRate: Float!
  commissionPercentage: Float!
  taxPercentage: Float!
  currency: String!
  isActive: Boolean!
  createdAt: DateTime!
  createdBy: User!
}

type Location {
  lat: Float!
  lng: Float!
}

type Rating {
  id: ID!
  booking: Booking!
  customer: User!
  driver: Driver!
  rating: Int!
  comment: String
  createdAt: DateTime!
}

type AuthPayload {
  token: String!
  user: User!
}

enum UserRole {
  CUSTOMER
  DRIVER
  OPERATOR
}

enum AuthProvider {
  GOOGLE
}

enum DriverStatus {
  PENDING
  ACTIVE
  SUSPENDED
}

enum BookingStatus {
  PENDING
  ASSIGNED
  PICKED_UP
  IN_TRANSIT
  DELIVERED
  COMPLETED
  CANCELLED
}

enum AssignmentType {
  AUTO_DISPATCH
  MANUAL_ASSIGNMENT
}

enum ParcelSize {
  SMALL
  MEDIUM
  LARGE
}

scalar DateTime
scalar JSON
```

#### Queries
```graphql
type Query {
  # Authentication
  me: User
  
  # Bookings
  booking(id: ID!): Booking
  myBookings(limit: Int, offset: Int): [Booking!]!
  fareEstimate(input: FareEstimateInput!): FareEstimate!
  
  # Driver
  availableJobs: [Booking!]!
  myActiveJob: Booking
  driverLocation(driverId: ID!): Location
  
  # Operator
  allBookings(search: String, status: BookingStatus, limit: Int, offset: Int): [Booking!]!
  allDrivers(status: DriverStatus): [Driver!]!
  activePricingConfiguration: PricingConfiguration
  bookingsByDriver(driverId: ID!, limit: Int, offset: Int): [Booking!]!
  bookingsByCustomer(customerId: ID!, limit: Int, offset: Int): [Booking!]!
}
```

#### Mutations
```graphql
type Mutation {
  # Authentication
  login(input: LoginInput!): AuthPayload!
  
  # Bookings
  createBooking(input: CreateBookingInput!): Booking!
  cancelBooking(id: ID!, reason: String!): Booking!
  
  # Driver
  acceptJob(bookingId: ID!, eta: Int): Booking!
  updateJobStatus(bookingId: ID!, status: BookingStatus!): Booking!
  updateDriverLocation(location: LocationInput!): Driver!
  
  # Rating
  submitRating(input: RatingInput!): Rating!
  
  # Operator
  createDriver(input: CreateDriverInput!): Driver!
  updateDriverStatus(driverId: ID!, status: DriverStatus!): Driver!
  assignJobToDriver(bookingId: ID!, driverId: ID!): Booking!
  updatePricingConfiguration(input: PricingConfigurationInput!): PricingConfiguration!
}
```

#### Input Types
```graphql
input LoginInput {
  authProvider: AuthProvider!
  authToken: String!
}

input CreateBookingInput {
  pickupLocation: LocationInput!
  pickupAddress: String!
  dropoffLocation: LocationInput!
  dropoffAddress: String!
  parcelSize: ParcelSize!
  assignmentType: AssignmentType = AUTO_DISPATCH
}

input LocationInput {
  lat: Float!
  lng: Float!
}

input FareEstimateInput {
  pickupLocation: LocationInput!
  dropoffLocation: LocationInput!
  parcelSize: ParcelSize!
}

input RatingInput {
  bookingId: ID!
  rating: Int!
  comment: String
}

input CreateDriverInput {
  name: String!
  email: String!
  vehiclePlate: String!
  idPhotoUrl: String
  bankInfo: JSON
}

input PricingConfigurationInput {
  baseFare: Float!
  perKmRate: Float!
  commissionPercentage: Float!
  taxPercentage: Float!
  currency: String!
}
```

### Example Queries

#### Get Fare Estimate
```graphql
query FareEstimate($input: FareEstimateInput!) {
  fareEstimate(input: $input) {
    fareAmount
    driverAmount
    commissionAmount
    taxAmount
    estimatedDistanceKm
    estimatedDurationMinutes
    currency
  }
}
```

#### Create Booking
```graphql
mutation CreateBooking($input: CreateBookingInput!) {
  createBooking(input: $input) {
    id
    status
    assignmentType
    fareAmount
    driverAmount
    pickupEta
    deliveryEta
  }
}
```

#### Accept Job (Driver)
```graphql
mutation AcceptJob($bookingId: ID!, $eta: Int) {
  acceptJob(bookingId: $bookingId, eta: $eta) {
    id
    status
    pickupEta
    driver {
      user {
        name
      }
    }
  }
}
```

#### Get Available Jobs (Driver)
```graphql
query AvailableJobs {
  availableJobs {
    id
    pickupAddress
    dropoffAddress
    fareAmount
    driverAmount
    estimatedDistanceKm
    estimatedDurationMinutes
    parcelSize
  }
}
```

#### Cancel Booking with Reason
```graphql
mutation CancelBooking($id: ID!, $reason: String!) {
  cancelBooking(id: $id, reason: $reason) {
    id
    status
    cancellationReason
  }
}
```

#### Submit Rating
```graphql
mutation SubmitRating($input: RatingInput!) {
  submitRating(input: $input) {
    id
    rating
    comment
    driver {
      ratingAverage
    }
  }
}
```

#### Operator: Create Driver
```graphql
mutation CreateDriver($input: CreateDriverInput!) {
  createDriver(input: $input) {
    id
    status
    user {
      name
      email
    }
    vehiclePlate
  }
}
```

#### Operator: Search Bookings
```graphql
query AllBookings($search: String, $status: BookingStatus) {
  allBookings(search: $search, status: $status) {
    id
    status
    customer {
      name
      email
    }
    driver {
      user {
        name
      }
      vehiclePlate
    }
    fareAmount
    createdAt
  }
}
```


