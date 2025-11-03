# Trip Organizer - Implementation Tasks

## Task Organization

Tasks are organized into phases with the following markers:
- **[P]** = Can be done in parallel with other [P] tasks in the same phase
- **[S]** = Sequential, must wait for previous tasks to complete
- **[T]** = Test task, should be completed before corresponding implementation

---

## Phase 1: Project Setup & Foundation

### Setup-1: Initialize Project Workspace [S]
**Description**: Create root workspace structure and initialize git repository  
**Files**: 
- `package.json` (root)
- `.gitignore`
- `README.md`

**Tasks**:
1. Create root directory `trip-organizer/`
2. Initialize root `package.json` with workspace configuration
3. Create comprehensive `.gitignore` for Node.js, Angular, and SQLite
4. Create initial `README.md` with project overview and setup instructions

**Verification**:
- [X] Root `package.json` exists with proper workspace structure
- [X] `.gitignore` includes node_modules, dist, database files, .env
- [X] README contains project description and getting started guide

---

### Setup-2: Initialize Angular Frontend [S]
**Description**: Create Angular 18 application with TypeScript strict mode  
**Files**: 
- `frontend/` directory
- `frontend/angular.json`
- `frontend/tsconfig.json`
- `frontend/package.json`

**Tasks**:
1. Run `ng new frontend --routing --style=scss --strict --ssr=false`
2. Configure `tsconfig.json` for strict mode and path aliases
3. Update `angular.json` for esbuild and production optimization
4. Install core dependencies: `@ngrx/store`, `@ngrx/effects`, `@ngrx/signals`, `@angular/cdk`

**Verification**:
- [X] Angular app runs with `ng serve`
- [X] TypeScript strict mode enabled
- [X] NgRx packages installed
- [X] SCSS compilation works

---

### Setup-3: Initialize NestJS Backend [S]
**Description**: Create NestJS backend application (BFF layer)  
**Files**: 
- `backend/` directory
- `backend/src/main.ts`
- `backend/tsconfig.json`
- `backend/package.json`

**Tasks**:
1. Run `nest new backend` or manually create NestJS structure
2. Install dependencies: `@nestjs/common`, `@nestjs/core`, `@nestjs/platform-express`
3. Install database: `better-sqlite3`, `typeorm`, `@nestjs/typeorm`
4. Install validation: `class-validator`, `class-transformer`
5. Configure TypeScript strict mode

**Verification**:
- [X] Backend starts with `npm run start:dev`
- [X] Server listens on port 3000
- [X] TypeORM configured
- [X] CORS enabled for localhost:4200

---

### Setup-4: Create Shared Types Package [P]
**Description**: Create shared TypeScript types for frontend and backend  
**Files**: 
- `shared/types/trip.types.ts`
- `shared/types/itinerary.types.ts`
- `shared/types/api-response.types.ts`
- `shared/tsconfig.json`

**Tasks**:
1. Create `shared/types/` directory structure
2. Define `Trip` interface
3. Define `ItineraryItemBase`, `Flight`, `Transport`, `Accommodation` interfaces
4. Define `Location` interface
5. Define `ItineraryGap` interface
6. Define `ApiResponse<T>` wrapper type
7. Configure TypeScript compilation for shared types

**Verification**:
- [X] All interfaces properly typed
- [X] Types compile without errors
- [X] Can be imported from both frontend and backend

---

### Setup-5: Configure ESLint and Prettier [P]
**Description**: Set up code quality tools  
**Files**: 
- `.eslintrc.json` (root)
- `.prettierrc` (root)
- `.prettierignore`

**Tasks**:
1. Install ESLint with Angular and TypeScript rules
2. Install Prettier and prettier-eslint
3. Configure ESLint rules for Angular best practices
4. Configure Prettier for consistent formatting
5. Add npm scripts for linting

**Verification**:
- [X] `npm run lint` works in frontend
- [X] `npm run lint` works in backend
- [X] Prettier formats code correctly
- [X] No conflicts between ESLint and Prettier

---

### Setup-6: Setup Database Schema and Migrations [S]
**Description**: Create SQLite database schema with TypeORM  
**Files**: 
- `backend/src/modules/database/data-source.ts`
- `backend/src/modules/database/migrations/001-initial-schema.ts`
- `backend/database/trip-organizer.db`

**Tasks**:
1. Configure TypeORM DataSource for SQLite
2. Create migration for `trips` table
3. Create migration for `itinerary_items` table
4. Create migration for `flights` table
5. Create migration for `transportation` table
6. Create migration for `accommodations` table
7. Create migration for `location_cache` table
8. Add indexes for performance
9. Run migrations

**Verification**:
- [X] Database file created
- [X] All tables created with correct schema
- [X] Indexes created
- [X] Foreign key constraints work
- [X] Can query tables successfully

---

### Setup-7: Configure Environment Variables [P]
**Description**: Set up environment configuration for both apps  
**Files**: 
- `frontend/src/environments/environment.ts`
- `frontend/src/environments/environment.prod.ts`
- `backend/.env.example`
- `backend/.env` (git-ignored)

**Tasks**:
1. Create frontend environment files with API URL
2. Create backend `.env.example` template
3. Create actual `.env` file (git-ignored)
4. Add Google Maps API key configuration
5. Add database path configuration
6. Add CORS origins configuration

**Verification**:
- [X] Frontend can access environment.apiUrl
- [X] Backend loads .env variables
- [X] .env is in .gitignore
- [X] .env.example has all required variables

---

## Phase 2: Core Models & Data Layer

### Data-1: Create Backend Trip Entity and Repository [S]
**Description**: Implement Trip entity with TypeORM  
**Files**: 
- `backend/src/modules/trips/entities/trip.entity.ts`
- `backend/src/modules/trips/trips.repository.ts`

**Tasks**:
1. Create `Trip` entity with TypeORM decorators
2. Define all fields (id, title, description, startDate, endDate, timestamps)
3. Create `TripsRepository` with CRUD methods
4. Implement `findAll()`, `findById()`, `create()`, `update()`, `delete()`

**Verification**:
- [X] Entity properly decorated
- [X] Repository methods work
- [X] Can create and retrieve trips from database
- [X] Timestamps auto-populate

---

### Data-2: Create Backend DTOs for Trips [P]
**Description**: Create Data Transfer Objects for validation  
**Files**: 
- `backend/src/modules/trips/dto/create-trip.dto.ts`
- `backend/src/modules/trips/dto/update-trip.dto.ts`

**Tasks**:
1. Create `CreateTripDto` with class-validator decorators
2. Create `UpdateTripDto` extending PartialType
3. Add validation rules (title required, dates valid, etc.)
4. Add date validation (endDate must be after startDate)

**Verification**:
- [X] DTOs have proper validation decorators
- [X] Invalid data throws validation errors
- [X] Date range validation works
- [X] Can transform plain objects to DTOs

---

### Data-3: Create Backend Itinerary Item Entities [S]
**Description**: Implement itinerary item entities with inheritance  
**Files**: 
- `backend/src/modules/itinerary/entities/itinerary-item.entity.ts`
- `backend/src/modules/itinerary/entities/flight.entity.ts`
- `backend/src/modules/itinerary/entities/transport.entity.ts`
- `backend/src/modules/itinerary/entities/accommodation.entity.ts`

**Tasks**:
1. Create base `ItineraryItem` entity
2. Create `Flight` entity with flight-specific fields
3. Create `Transport` entity with transport-specific fields
4. Create `Accommodation` entity with accommodation-specific fields
5. Set up proper relationships with Trip entity
6. Configure JSON columns for Location data

**Verification**:
- [X] All entities properly decorated
- [X] Relationships work correctly
- [X] JSON columns parse/stringify correctly
- [X] Can query items with their trips

---

### Data-4: Create Backend Itinerary Repositories [S]
**Description**: Implement repositories for itinerary items  
**Files**: 
- `backend/src/modules/itinerary/itinerary.repository.ts`

**Tasks**:
1. Create repository methods for flights
2. Create repository methods for transport
3. Create repository methods for accommodations
4. Implement `findByTripId()` with proper sorting
5. Implement `findById()` with type checking
6. Implement `create()` for each type
7. Implement `update()` and `delete()`
8. Implement `reorder()` for drag-drop

**Verification**:
- [X] Can create all item types
- [X] Can retrieve items by trip ID
- [X] Items sorted by startDate
- [X] Can update and delete items
- [X] Reorder updates dates correctly

---

### Data-5: Create Frontend TypeScript Models [P] ✅
**Description**: Create frontend model classes and interfaces  
**Files**: 
- `frontend/src/app/core/models/trip.model.ts`
- `frontend/src/app/core/models/itinerary-item.model.ts`
- `frontend/src/app/core/models/flight.model.ts`
- `frontend/src/app/core/models/transport.model.ts`
- `frontend/src/app/core/models/accommodation.model.ts`
- `frontend/src/app/core/models/location.model.ts`

**Tasks**:
1. Create `Trip` model matching shared types
2. Create `ItineraryItemBase` abstract model
3. Create `Flight` model extending base
4. Create `Transport` model extending base
5. Create `Accommodation` model extending base
6. Create `Location` model
7. Add utility methods (duration calculation, validation, etc.)

**Verification**:
- [X] Models match shared types
- [X] Type safety enforced
- [X] Utility methods work correctly
- [X] Can instantiate and use models

---

## Phase 3: Backend API Implementation

### API-1: Implement Trips Service [S] ✅
**Description**: Create business logic for trip management  
**Files**: 
- `backend/src/modules/trips/trips.service.ts`

**Tasks**:
1. Create `TripsService` with dependency injection
2. Implement `findAll()` method
3. Implement `findOne()` method with error handling
4. Implement `create()` with validation
5. Implement `update()` with validation
6. Implement `remove()` with cascade handling
7. Add date validation logic
8. Add error handling for not found scenarios

**Verification**:
- [X] All CRUD operations work
- [X] Validation errors thrown correctly
- [X] Not found errors handled
- [X] Deleting trip cascades to items

---

### API-2: Implement Trips Controller [S] ✅
**Description**: Create REST API endpoints for trips  
**Files**: 
- `backend/src/modules/trips/trips.controller.ts`

**Tasks**:
1. Create `TripsController` with route prefix `/api/trips`
2. Implement `GET /api/trips` endpoint
3. Implement `GET /api/trips/:id` endpoint
4. Implement `POST /api/trips` endpoint
5. Implement `PUT /api/trips/:id` endpoint
6. Implement `DELETE /api/trips/:id` endpoint
7. Add validation pipes
8. Add error handling and proper HTTP status codes

**Verification**:
- [X] All endpoints respond correctly
- [X] Status codes appropriate (200, 201, 404, 400)
- [X] DTOs validated
- [X] Returns proper JSON responses

---

### API-3: Implement Itinerary Service [S] ✅
**Description**: Create business logic for itinerary items  
**Files**: 
- `backend/src/modules/itinerary/itinerary.service.ts`

**Tasks**:
1. Create `ItineraryService` with repository injection
2. Implement `findByTripId()` with chronological sorting
3. Implement `findOne()` with type checking
4. Implement `createFlight()`, `createTransport()`, `createAccommodation()`
5. Implement `update()` with type validation
6. Implement `remove()` method
7. Implement `reorder()` for drag-drop updates
8. Add validation for date/time logic
9. Calculate duration automatically

**Verification**:
- [X] Can create all item types
- [X] Items returned in chronological order
- [X] Duration calculated correctly
- [X] Reorder updates timestamps
- [X] Validation prevents invalid data

---

### API-4: Implement Itinerary Controller [S] ✅
**Description**: Create REST API endpoints for itinerary items  
**Files**: 
- `backend/src/modules/itinerary/itinerary.controller.ts`

**Tasks**:
1. Create `ItineraryController` with routes
2. Implement `GET /api/trips/:tripId/itinerary` endpoint
3. Implement `GET /api/itinerary/:id` endpoint
4. Implement `POST /api/trips/:tripId/itinerary/flight` endpoint
5. Implement `POST /api/trips/:tripId/itinerary/transport` endpoint
6. Implement `POST /api/trips/:tripId/itinerary/accommodation` endpoint
7. Implement `PUT /api/itinerary/:id` endpoint
8. Implement `DELETE /api/itinerary/:id` endpoint
9. Implement `PATCH /api/itinerary/:id/reorder` endpoint

**Verification**:
- [X] All endpoints respond correctly
- [X] Can create each item type via API
- [X] Can retrieve items for a trip
- [X] Can update and delete items
- [X] Reorder endpoint works

---

### API-5: Implement Gap Detection Service [S] ✅
**Description**: Create algorithm to detect itinerary gaps  
**Files**: 
- `backend/src/modules/itinerary/gap-detection.service.ts`

**Tasks**:
1. Create `GapDetectionService`
2. Implement `detectGaps(items: ItineraryItem[]): ItineraryGap[]`
3. Implement time gap detection (> 2 hours between items)
4. Implement location mismatch detection (arrival ≠ next departure)
5. Implement missing accommodation detection (overnight without lodging)
6. Calculate gap duration
7. Assign severity levels (info, warning, error)
8. Generate helpful gap messages and suggestions

**Verification**:
- [X] Detects time gaps correctly
- [X] Detects location mismatches
- [X] Detects missing accommodations
- [X] Severity levels assigned appropriately
- [X] Messages are clear and helpful

---

### API-6: Implement Gap Detection Controller [S]
**Description**: Create API endpoint for gap detection  
**Files**: 
- `backend/src/modules/itinerary/gap-detection.controller.ts`

**Tasks**:
1. Add gap detection route to itinerary module
2. Implement `GET /api/trips/:tripId/gaps` endpoint
3. Retrieve all items for trip
4. Run gap detection algorithm
5. Return gaps with proper formatting

**Verification**:
- [X] Endpoint returns gaps for trip
- [X] Gaps correctly identified
- [X] Response includes all gap details
- [X] Performance acceptable (<500ms)

---

### API-7: Implement Maps Service (BFF Proxy) [S]
**Description**: Create proxy service for Google Maps API  
**Files**: 
- `backend/src/modules/maps/maps.service.ts`
- `backend/src/modules/maps/geocoding.service.ts`
- `backend/src/modules/maps/location-cache.repository.ts`

**Tasks**:
1. Install `@googlemaps/google-maps-services-js`
2. Create `MapsService` with API client
3. Create `GeocodingService` with caching
4. Implement `geocode(address: string)` method
5. Check cache before making API call
6. Store geocoding results in location_cache table
7. Implement `getPlaceDetails(placeId: string)` method
8. Add rate limiting to prevent API abuse
9. Handle API errors gracefully

**Verification**:
- [X] Geocoding works for valid addresses
- [X] Results cached in database
- [X] Cache hit returns cached result
- [X] Invalid addresses handled gracefully
- [X] Rate limiting prevents abuse

---

### API-8: Implement Maps Controller [S]
**Description**: Create API endpoints for map operations  
**Files**: 
- `backend/src/modules/maps/maps.controller.ts`

**Tasks**:
1. Create `MapsController` with `/api/maps` prefix
2. Implement `POST /api/maps/geocode` endpoint
3. Implement `GET /api/maps/place/:placeId` endpoint
4. Add request validation
5. Return standardized Location objects
6. Add error handling for API failures

**Verification**:
- [X] Geocode endpoint returns coordinates
- [X] Place details endpoint works
- [X] Errors handled properly
- [X] API key never exposed to frontend

---
3. Retrieve all items for trip
4. Run gap detection algorithm
5. Return gaps with proper formatting

**Verification**:
- [X] Endpoint returns gaps for trip
- [X] Gaps correctly identified
- [X] Response includes all gap details
- [X] Performance acceptable (<500ms)

---

### API-7: Implement Maps Service (BFF Proxy) [S]
**Description**: Create proxy service for Google Maps API  
**Files**: 
- `backend/src/modules/maps/maps.service.ts`
- `backend/src/modules/maps/geocoding.service.ts`
- `backend/src/modules/maps/location-cache.repository.ts`

**Tasks**:
1. Install `@googlemaps/google-maps-services-js`
2. Create `MapsService` with API client
3. Create `GeocodingService` with caching
4. Implement `geocode(address: string)` method
5. Check cache before making API call
6. Store geocoding results in location_cache table
7. Implement `getPlaceDetails(placeId: string)` method
8. Add rate limiting to prevent API abuse
9. Handle API errors gracefully

**Verification**:
- [X] Geocoding works for valid addresses
- [X] Results cached in database
- [X] Cache hit returns cached result
- [X] Invalid addresses handled gracefully
- [X] Rate limiting prevents abuse

---

### API-8: Implement Maps Controller [S]
**Description**: Create API endpoints for map operations  
**Files**: 
- `backend/src/modules/maps/maps.controller.ts`

**Tasks**:
1. Create `MapsController` with `/api/maps` prefix
2. Implement `POST /api/maps/geocode` endpoint
3. Implement `GET /api/maps/place/:placeId` endpoint
4. Add request validation
5. Return standardized Location objects
6. Add error handling for API failures

**Verification**:
- [ ] Geocode endpoint returns coordinates
- [ ] Place details endpoint works
- [ ] Errors handled properly
- [ ] API key never exposed to frontend

---

## Phase 4: Frontend State Management

### State-1: Setup NgRx Store Structure [S]
**Description**: Configure root NgRx store with signals  
**Files**: 
- `frontend/src/app/store/app.state.ts`
- `frontend/src/app/store/index.ts`
- `frontend/src/app/app.config.ts`

**Tasks**:
1. Define `AppState` interface
2. Configure `provideStore()` in app config
3. Configure `provideEffects()` in app config
4. Set up Redux DevTools
5. Create store module barrel export

**Verification**:
- [X] Store initialized in application
- [X] Redux DevTools connected
- [X] No console errors
- [X] Store accessible via inject()

---

### State-2: Create Trips Store (NgRx Signals) [S]
**Description**: Implement trips feature store with signals  
**Files**: 
- `frontend/src/app/features/trips/store/trips.store.ts`

**Tasks**:
1. Create `TripsStore` using `signalStore()`
2. Define `TripsState` interface
3. Implement initial state
4. Add `withState()` for trips array, loading, error, selectedTripId
5. Add `withComputed()` for selectedTrip, tripCount
6. Add `withMethods()` for loadTrips, selectTrip, createTrip, updateTrip, deleteTrip
7. Integrate with TripsService (API calls)
8. Handle loading and error states

**Verification**:
- [X] Store methods work correctly
- [X] Signals update reactively
- [X] Computed signals derive correctly
- [X] API integration works
- [X] Loading and error states managed

---

### State-3: Create Itinerary Store (NgRx Signals) [S]
**Description**: Implement itinerary feature store with signals  
**Files**: 
- `frontend/src/app/features/itinerary/store/itinerary.store.ts`

**Tasks**:
1. Create `ItineraryStore` using `signalStore()`
2. Define `ItineraryState` interface
3. Implement initial state
4. Add items array, gaps array, selectedItemId, loading, error states
5. Add computed signals for sorted items, gap count
6. Add methods for loadItems, createItem, updateItem, deleteItem, reorderItems
7. Add method for loadGaps
8. Integrate with ItineraryService

**Verification**:
- [X] Store methods work correctly
- [X] Items sorted chronologically
- [X] Gaps loaded and tracked
- [X] Reorder updates state
- [X] API integration works

---

### State-4: Create API Service (Frontend) [S]
**Description**: Create HTTP client service for backend API  
**Files**: 
- `frontend/src/app/core/services/api.service.ts`
- `frontend/src/app/core/interceptors/api.interceptor.ts`

**Tasks**:
1. Create `ApiService` with HttpClient
2. Implement base URL configuration from environment
3. Create generic `get<T>()`, `post<T>()`, `put<T>()`, `delete<T>()` methods
4. Create `ApiInterceptor` for common headers
5. Add error handling interceptor
6. Add loading state management
7. Type all requests with proper interfaces

**Verification**:
- [X] Can make API requests
- [X] Base URL prepended correctly
- [X] Error handling works
- [X] Type safety enforced
- [X] Interceptors apply correctly

---

### State-5: Create Trips API Service [P]
**Description**: Create service for trip-related API calls  
**Files**: 
- `frontend/src/app/features/trips/services/trips-api.service.ts`

**Tasks**:
1. Create `TripsApiService` using ApiService
2. Implement `getTrips(): Observable<Trip[]>`
3. Implement `getTrip(id: string): Observable<Trip>`
4. Implement `createTrip(trip: Partial<Trip>): Observable<Trip>`
5. Implement `updateTrip(id: string, trip: Partial<Trip>): Observable<Trip>`
6. Implement `deleteTrip(id: string): Observable<void>`
7. Add proper error handling
8. Add response transformation if needed

**Verification**:
- [X] All methods return correctly typed Observables
- [X] API endpoints called correctly
- [X] Errors propagated properly
- [X] Can be injected into store

---

### State-6: Create Itinerary API Service [P]
**Description**: Create service for itinerary-related API calls  
**Files**: 
- `frontend/src/app/features/itinerary/services/itinerary-api.service.ts`

**Tasks**:
1. Create `ItineraryApiService` using ApiService
2. Implement `getItems(tripId: string): Observable<ItineraryItem[]>`
3. Implement `getItem(id: string): Observable<ItineraryItem>`
4. Implement `createFlight()`, `createTransport()`, `createAccommodation()`
5. Implement `updateItem(id: string, item: Partial<ItineraryItem>)`
6. Implement `deleteItem(id: string)`
7. Implement `reorderItem(id: string, newDate: Date)`
8. Implement `getGaps(tripId: string): Observable<ItineraryGap[]>`

**Verification**:
- [X] All CRUD operations work
- [X] Type-specific creation methods work
- [X] Gaps endpoint returns correctly
- [X] Reorder API call works

---

### State-7: Create Maps Service (Frontend) [P]
**Description**: Create frontend service for maps functionality  
**Files**: 
- `frontend/src/app/features/maps/services/maps.service.ts`

**Tasks**:
1. Create `MapsService` using ApiService
2. Implement `geocodeAddress(address: string): Observable<Location>`
3. Implement `getPlaceDetails(placeId: string): Observable<Location>`
4. Implement `getMapUrl(location: Location): string` for Google Maps links
5. Implement `getDirectionsUrl(from: Location, to: Location): string`
6. Add error handling for failed geocoding

**Verification**:
- [X] Geocoding returns Location objects
- [X] Map URLs generated correctly
- [X] Directions URLs work
- [X] Errors handled gracefully

---

## Phase 5: Core UI Components

### UI-1: Create App Shell and Navigation [S]
**Description**: Implement main app layout and navigation  
**Files**: 
- `frontend/src/app/app.component.ts`
- `frontend/src/app/app.component.scss`
- `frontend/src/app/app.routes.ts`

**Tasks**:
1. Update `AppComponent` with basic layout
2. Add navigation header with app title
3. Configure router outlet
4. Set up lazy-loaded routes for features
5. Add basic SCSS styling
6. Configure route guards if needed

**Verification**:
- [X] App loads without errors
- [X] Navigation renders
- [X] Router outlet displays routed components
- [X] Lazy loading works

---

### UI-2: Create SCSS Theme and Variables [P]
**Description**: Set up global styles and theme  
**Files**: 
- `frontend/src/assets/styles/_variables.scss`
- `frontend/src/assets/styles/_mixins.scss`
- `frontend/src/assets/styles/_themes.scss`
- `frontend/src/styles.scss`

**Tasks**:
1. Define color variables (primary, accent, warn)
2. Define item type colors (flight, transport, accommodation)
3. Define spacing scale
4. Create card-style mixin
5. Create responsive-grid mixin
6. Create typography scale
7. Import into main styles.scss
8. Set up mobile-first breakpoints

**Verification**:
- [X] Variables accessible in component styles
- [X] Mixins work correctly
- [X] Theme colors consistent
- [X] Responsive breakpoints work

---

### UI-3: Create Shared Components Module [S]
**Description**: Set up shared components structure  
**Files**: 
- `frontend/src/app/shared/components/loading-spinner/`
- `frontend/src/app/shared/components/confirmation-dialog/`

**Tasks**:
1. Create `LoadingSpinnerComponent`
2. Create `ConfirmationDialogComponent`
3. Make components standalone
4. Style components with SCSS
5. Add proper accessibility attributes

**Verification**:
- [X] Components can be imported
- [X] Spinner shows loading state
- [X] Dialog shows and accepts/cancels
- [X] ARIA attributes present

---

### UI-4: Create Date/Time Utilities and Pipes [P]
**Description**: Create utilities for date handling  
**Files**: 
- `frontend/src/app/core/utils/date.utils.ts`
- `frontend/src/app/shared/pipes/duration.pipe.ts`
- `frontend/src/app/shared/pipes/date-format.pipe.ts`

**Tasks**:
1. Create `DateUtils` with helper functions
2. Implement `calculateDuration(start: Date, end: Date): number`
3. Implement `formatDuration(minutes: number): string`
4. Implement `isValidDateRange(start: Date, end: Date): boolean`
5. Create `DurationPipe` for displaying durations
6. Create `DateFormatPipe` for consistent date formatting
7. Add timezone handling utilities

**Verification**:
- [X] Duration calculation accurate
- [X] Pipes transform correctly
- [X] Date validation works
- [X] Timezone handling correct

---

### UI-5: Create Trip List Component [S]
**Description**: Display list of trips in dashboard  
**Files**: 
- `frontend/src/app/features/trips/components/trip-list/trip-list.component.ts`
- `frontend/src/app/features/trips/components/trip-list/trip-list.component.scss`
- `frontend/src/app/features/trips/components/trip-list/trip-list.component.html`

**Tasks**:
1. Create standalone `TripListComponent`
2. Inject `TripsStore`
3. Display trips using @for loop
4. Show trip title, dates, and item count
5. Add click handler to navigate to trip detail
6. Add delete button with confirmation
7. Style with grid layout
8. Add empty state when no trips

**Verification**:
- [X] Trips displayed in list
- [X] Click navigates to detail
- [X] Delete works with confirmation
- [X] Empty state shows when no trips
- [X] Responsive layout works

---

### UI-6: Create Trip Card Component [P]
**Description**: Display individual trip as a card  
**Files**: 
- `frontend/src/app/features/trips/components/trip-card/trip-card.component.ts`
- `frontend/src/app/features/trips/components/trip-card/trip-card.component.scss`
- `frontend/src/app/features/trips/components/trip-card/trip-card.component.html`

**Tasks**:
1. Create standalone `TripCardComponent`
2. Add `@Input() trip: Trip`
3. Add `@Output() delete` and `@Output() select` events
4. Display trip information attractively
5. Add action buttons
6. Style with card mixin
7. Add hover effects

**Verification**:
- [x] Card displays trip info
- [x] Events emit correctly
- [x] Styling matches design
- [x] Hover effects work

---

### UI-7: Create Trip Form Component [S]
**Description**: Form for creating/editing trips  
**Files**: 
- `frontend/src/app/features/trips/components/trip-form/trip-form.component.ts`
- `frontend/src/app/features/trips/components/trip-form/trip-form.component.scss`
- `frontend/src/app/features/trips/components/trip-form/trip-form.component.html`

**Tasks**:
1. Create standalone `TripFormComponent`
2. Create reactive form with FormBuilder
3. Add fields: title, description, startDate, endDate
4. Add validation (required, date range)
5. Add custom validator for date range
6. Add submit and cancel handlers
7. Emit form values to parent
8. Style form with proper spacing

**Verification**:
- [x] Form renders correctly
- [x] Validation works
- [x] Date range validation prevents invalid dates
- [x] Submit emits valid trip data
- [x] Cancel handler works

---

### UI-8: Create Trips Dashboard Page [S]
**Description**: Main page for viewing all trips  
**Files**: 
- `frontend/src/app/features/trips/pages/trips-dashboard/trips-dashboard.component.ts`
- `frontend/src/app/features/trips/pages/trips-dashboard/trips-dashboard.component.scss`
- `frontend/src/app/features/trips/pages/trips-dashboard/trips-dashboard.component.html`

**Tasks**:
1. Create smart component `TripsDashboardComponent`
2. Inject `TripsStore` and load trips on init
3. Use `TripListComponent` to display trips
4. Add "Create New Trip" button
5. Handle create trip flow (show form dialog)
6. Handle trip selection (navigate to detail)
7. Handle trip deletion
8. Show loading and error states

**Verification**:
- [x] Dashboard loads trips
- [x] Can create new trip
- [x] Can select trip to view details
- [x] Can delete trip
- [x] Loading spinner shows
- [x] Error messages display

---

### UI-9: Create Trip Detail Page [S]
**Description**: Page showing trip details and itinerary  
**Files**: 
- `frontend/src/app/features/trips/pages/trip-detail/trip-detail.component.ts`
- `frontend/src/app/features/trips/pages/trip-detail/trip-detail.component.scss`
- `frontend/src/app/features/trips/pages/trip-detail/trip-detail.component.html`

**Tasks**:
1. Create smart component `TripDetailComponent`
2. Get trip ID from route params
3. Load trip and itinerary items from stores
4. Display trip header with title and dates
5. Display itinerary timeline
6. Add "Edit Trip" button
7. Add "Add Item" button
8. Show loading and error states

**Verification**:
- [X] Loads trip by ID
- [X] Displays trip info
- [X] Shows itinerary items
- [X] Edit and add buttons work
- [X] 404 handling for invalid IDs

---

### UI-10: Create Itinerary Timeline Component [S]
**Description**: Visual timeline of itinerary items  
**Files**: 
- `frontend/src/app/features/itinerary/components/itinerary-timeline/itinerary-timeline.component.ts`
- `frontend/src/app/features/itinerary/components/itinerary-timeline/itinerary-timeline.component.scss`
- `frontend/src/app/features/itinerary/components/itinerary-timeline/itinerary-timeline.component.html`

**Tasks**:
1. Create `ItineraryTimelineComponent`
2. Add `@Input() items: ItineraryItem[]`
3. Add `@Input() gaps: ItineraryGap[]`
4. Group items by date
5. Display items chronologically
6. Show gaps between items
7. Color-code by item type
8. Make items clickable to edit

**Verification**:
- [X] Items displayed chronologically
- [X] Grouped by day
- [X] Gaps shown visually
- [X] Color coding works
- [X] Click opens edit

---

### UI-11: Create Itinerary Item Component [P]
**Description**: Display single itinerary item  
**Files**: 
- `frontend/src/app/features/itinerary/components/itinerary-item/itinerary-item.component.ts`
- `frontend/src/app/features/itinerary/components/itinerary-item/itinerary-item.component.scss`
- `frontend/src/app/features/itinerary/components/itinerary-item/itinerary-item.component.html`

**Tasks**:
1. Create `ItineraryItemComponent`
2. Add `@Input() item: ItineraryItem`
3. Add `@Output() edit` and `@Output() delete` events
4. Display different views based on item type
5. Show times, locations, durations
6. Add action buttons
7. Style with card and border color by type

**Verification**:
- [X] Displays flight info correctly
- [X] Displays transport info correctly
- [X] Displays accommodation info correctly
- [X] Actions emit events
- [X] Styling correct per type

---

### UI-12: Create Gap Indicator Component [P]
**Description**: Visual indicator for itinerary gaps  
**Files**: 
- `frontend/src/app/features/itinerary/components/gap-indicator/gap-indicator.component.ts`
- `frontend/src/app/features/itinerary/components/gap-indicator/gap-indicator.component.scss`
- `frontend/src/app/features/itinerary/components/gap-indicator/gap-indicator.component.html`

**Tasks**:
1. Create `GapIndicatorComponent`
2. Add `@Input() gap: ItineraryGap`
3. Display gap type icon
4. Display gap duration
5. Display gap message
6. Color-code by severity
7. Add "Fill Gap" button
8. Style with warning/error colors

**Verification**:
- [X] Shows gap information clearly
- [X] Severity colors work
- [X] Icons appropriate
- [X] Fill gap button emits event

---

## Phase 6: Item Forms & CRUD

### Form-1: Create Flight Form Component [S]
**Description**: Form for creating/editing flights  
**Files**: 
- `frontend/src/app/features/itinerary/components/flight-form/flight-form.component.ts`
- `frontend/src/app/features/itinerary/components/flight-form/flight-form.component.scss`
- `frontend/src/app/features/itinerary/components/flight-form/flight-form.component.html`

**Tasks**:
1. Create `FlightFormComponent` with reactive form
2. Add fields: flightNumber, airline, departure/arrival locations, times
3. Add location search with geocoding
4. Calculate duration automatically with signals
5. Add validation (times, locations required)
6. Add date/time pickers
7. Emit form data on submit

**Verification**:
- [X] Form validates correctly
- [X] Duration auto-calculates
- [X] Location search works
- [X] Date/time pickers work
- [X] Submit emits valid flight data

---

### Form-2: Create Transport Form Component [S]
**Description**: Form for creating/editing transportation  
**Files**: 
- `frontend/src/app/features/itinerary/components/transport-form/transport-form.component.ts`
- `frontend/src/app/features/itinerary/components/transport-form/transport-form.component.scss`
- `frontend/src/app/features/itinerary/components/transport-form/transport-form.component.html`

**Tasks**:
1. Create `TransportFormComponent` with reactive form
2. Add transport type selector (train, bus, car, etc.)
3. Add departure/arrival locations and times
4. Add provider field (optional)
5. Calculate duration automatically
6. Add validation
7. Reuse location search component

**Verification**:
- [X] Transport type selection works
- [X] All fields validate
- [X] Duration auto-calculates
- [X] Location search integrated
- [X] Submit emits valid data

---

### Form-3: Create Accommodation Form Component [S]
**Description**: Form for creating/editing accommodations  
**Files**: 
- `frontend/src/app/features/itinerary/components/accommodation-form/accommodation-form.component.ts`
- `frontend/src/app/features/itinerary/components/accommodation-form/accommodation-form.component.scss`
- `frontend/src/app/features/itinerary/components/accommodation-form/accommodation-form.component.html`

**Tasks**:
1. Create `AccommodationFormComponent` with reactive form
2. Add fields: name, type, address, check-in/out times
3. Add phone number field (optional)
4. Add notes field
5. Add location search/validation
6. Calculate duration automatically
7. Add validation

**Verification**:
- [X] All fields render correctly
- [X] Location integration works
- [X] Duration calculated
- [X] Validation works
- [X] Submit emits valid data

---

### Form-4: Create Location Search Component [P]
**Description**: Reusable location search with geocoding  
**Files**: 
- `frontend/src/app/shared/components/location-search/location-search.component.ts`
- `frontend/src/app/shared/components/location-search/location-search.component.scss`
- `frontend/src/app/shared/components/location-search/location-search.component.html`

**Tasks**:
1. Create standalone `LocationSearchComponent`
2. Implement ControlValueAccessor for form integration
3. Add text input for address
4. Call geocoding service on input/search
5. Display geocoding results as suggestions
6. Emit selected Location object
7. Add loading state while geocoding
8. Handle geocoding errors

**Verification**:
- [ ] Works as form control
- [ ] Geocoding triggered correctly
- [ ] Suggestions displayed
- [ ] Selection emits Location
- [ ] Errors handled gracefully

---

### Form-5: Create Date/Time Picker Component [P]
**Description**: Reusable date and time picker  
**Files**: 
- `frontend/src/app/shared/components/date-time-picker/date-time-picker.component.ts`
- `frontend/src/app/shared/components/date-time-picker/date-time-picker.component.scss`
- `frontend/src/app/shared/components/date-time-picker/date-time-picker.component.html`

**Tasks**:
1. Create standalone `DateTimePickerComponent`
2. Implement ControlValueAccessor
3. Add native date and time inputs
4. Combine date and time into single Date object
5. Add min/max constraints support
6. Style inputs consistently
7. Add proper labels and accessibility

**Verification**:
- [ ] Works as form control
- [ ] Date and time combine correctly
- [ ] Min/max constraints enforced
- [ ] Accessible with keyboard
- [ ] Value updates correctly

---

### Form-6: Create Add Item Page [S]
**Description**: Page for adding new itinerary items  
**Files**: 
- `frontend/src/app/features/itinerary/pages/add-item/add-item.component.ts`
- `frontend/src/app/features/itinerary/pages/add-item/add-item.component.scss`
- `frontend/src/app/features/itinerary/pages/add-item/add-item.component.html`

**Tasks**:
1. Create smart component `AddItemComponent`
2. Get trip ID from route params
3. Add item type selector (flight, transport, accommodation)
4. Show appropriate form based on selection
5. Handle form submission
6. Call store to create item
7. Navigate back to trip detail on success
8. Show errors if creation fails

**Verification**:
- [ ] Type selector works
- [ ] Correct form shown per type
- [ ] Item created successfully
- [ ] Navigation works
- [ ] Errors displayed

---

### Form-7: Implement Edit Item Functionality [S]
**Description**: Add edit capability to existing items  
**Files**: 
- `frontend/src/app/features/itinerary/pages/add-item/add-item.component.ts` (extend)

**Tasks**:
1. Add support for edit mode (get item ID from route)
2. Load existing item data
3. Pre-populate form with item data
4. Update instead of create on submit
5. Show "Edit" instead of "Create" in UI
6. Handle update in store

**Verification**:
- [ ] Loads existing item for edit
- [ ] Form pre-populated correctly
- [ ] Update saves changes
- [ ] Navigation works
- [ ] Original item updated

---

## Phase 7: Advanced Features

### Advanced-1: Implement Drag-and-Drop [S]
**Description**: Add drag-and-drop to timeline for reordering  
**Files**: 
- `frontend/src/app/features/itinerary/components/drag-drop-list/drag-drop-list.component.ts`
- `frontend/src/app/features/itinerary/components/drag-drop-list/drag-drop-list.component.scss`
- `frontend/src/app/features/itinerary/components/drag-drop-list/drag-drop-list.component.html`

**Tasks**:
1. Install @angular/cdk/drag-drop
2. Create `DragDropListComponent`
3. Add `cdkDropList` and `cdkDrag` directives
4. Handle `cdkDropListDropped` event
5. Update item dates based on new position
6. Call store reorder method
7. Add visual feedback during drag
8. Trigger gap detection after reorder

**Verification**:
- [ ] Items can be dragged
- [ ] Drop updates order
- [ ] Dates recalculated correctly
- [ ] Visual feedback during drag
- [ ] Gaps re-detected after reorder

---

### Advanced-2: Integrate Drag-Drop into Timeline [S]
**Description**: Replace static timeline with drag-drop version  
**Files**: 
- `frontend/src/app/features/itinerary/components/itinerary-timeline/itinerary-timeline.component.ts` (update)

**Tasks**:
1. Replace item list with `DragDropListComponent`
2. Pass items to drag-drop list
3. Handle reorder events
4. Update store on reorder
5. Re-fetch gaps after reorder
6. Maintain grouping by day during drag

**Verification**:
- [ ] Timeline supports drag-drop
- [ ] Reorder persists to backend
- [ ] Gaps update after reorder
- [ ] Day grouping maintained
- [ ] Smooth UX

---

### Advanced-3: Create Location Display Component [P]
**Description**: Component to display location with map link  
**Files**: 
- `frontend/src/app/features/maps/components/location-display/location-display.component.ts`
- `frontend/src/app/features/maps/components/location-display/location-display.component.scss`
- `frontend/src/app/features/maps/components/location-display/location-display.component.html`

**Tasks**:
1. Create `LocationDisplayComponent`
2. Add `@Input() location: Location`
3. Display address and city
4. Add button to view on Google Maps
5. Generate Google Maps URL using MapsService
6. Open link in new tab
7. Add map icon

**Verification**:
- [ ] Location displayed clearly
- [ ] Map link works
- [ ] Opens in new tab
- [ ] Icon shows correctly

---

### Advanced-4: Create Trip Map View (Optional) [P]
**Description**: Show all trip locations on a map  
**Files**: 
- `frontend/src/app/features/maps/components/trip-map-view/trip-map-view.component.ts`
- `frontend/src/app/features/maps/components/trip-map-view/trip-map-view.component.scss`
- `frontend/src/app/features/maps/components/trip-map-view/trip-map-view.component.html`

**Tasks**:
1. Create `TripMapViewComponent`
2. Add `@Input() items: ItineraryItem[]`
3. Extract all locations from items
4. Generate static Google Maps URL with multiple markers
5. Display map iframe or image
6. Add toggle to switch between timeline and map view
7. Mark locations with numbered pins

**Verification**:
- [ ] Map displays all locations
- [ ] Markers numbered correctly
- [ ] Map view toggles with timeline
- [ ] Handles items without locations

---

### Advanced-5: Implement Auto-Save [S]
**Description**: Automatically save changes  
**Files**: 
- `frontend/src/app/core/services/auto-save.service.ts`

**Tasks**:
1. Create `AutoSaveService`
2. Listen to form value changes
3. Debounce changes (wait 2 seconds after last change)
4. Auto-save to backend
5. Show save indicator (saving/saved)
6. Handle save errors
7. Integrate into trip and item forms

**Verification**:
- [ ] Changes auto-save after delay
- [ ] Indicator shows save status
- [ ] Multiple rapid changes debounced
- [ ] Errors shown to user

---

### Advanced-6: Implement Export to JSON [S]
**Description**: Export trip data as JSON file  
**Files**: 
- `backend/src/modules/trips/trips.controller.ts` (add export endpoint)
- `frontend/src/app/features/trips/pages/trip-detail/trip-detail.component.ts` (add export button)

**Tasks**:
1. Create `GET /api/trips/:id/export/json` endpoint
2. Serialize trip with all items
3. Return JSON with proper formatting
4. Add "Export" button to trip detail page
5. Download JSON file on click
6. Name file appropriately (trip-title-date.json)

**Verification**:
- [ ] Export endpoint returns complete data
- [ ] File downloads correctly
- [ ] JSON is valid and readable
- [ ] All data included

---

### Advanced-7: Implement Export to iCal [S]
**Description**: Export trip as iCalendar file  
**Files**: 
- `backend/src/modules/trips/export.service.ts`
- `backend/src/modules/trips/trips.controller.ts` (add ical endpoint)

**Tasks**:
1. Install `ical-generator` package
2. Create `ExportService`
3. Convert trip items to iCal events
4. Create `GET /api/trips/:id/export/ical` endpoint
5. Return .ics file with proper MIME type
6. Add "Add to Calendar" button to frontend
7. Download .ics file on click

**Verification**:
- [ ] iCal file generated correctly
- [ ] Can import into calendar apps
- [ ] All events included
- [ ] Times and locations correct

---

## Phase 8: Testing & Polish

### Test-1: Write Unit Tests for Backend Services [T]
**Description**: Test backend business logic  
**Files**: 
- `backend/src/modules/trips/trips.service.spec.ts`
- `backend/src/modules/itinerary/itinerary.service.spec.ts`
- `backend/src/modules/itinerary/gap-detection.service.spec.ts`

**Tasks**:
1. Set up Jest test environment
2. Write tests for TripsService CRUD operations
3. Write tests for ItineraryService CRUD operations
4. Write comprehensive tests for gap detection algorithm
5. Test edge cases and error scenarios
6. Mock repository dependencies
7. Aim for >80% coverage

**Verification**:
- [ ] All tests pass
- [ ] Coverage >80%
- [ ] Edge cases covered
- [ ] Error scenarios tested

---

### Test-2: Write Unit Tests for Frontend Components [T]
**Description**: Test Angular components  
**Files**: 
- `frontend/src/app/features/trips/components/**/*.spec.ts`
- `frontend/src/app/features/itinerary/components/**/*.spec.ts`

**Tasks**:
1. Set up Jest for Angular
2. Write tests for TripListComponent
3. Write tests for TripFormComponent
4. Write tests for timeline components
5. Write tests for form components
6. Test component inputs, outputs, and behavior
7. Test with TestBed and component fixtures

**Verification**:
- [ ] All component tests pass
- [ ] User interactions tested
- [ ] Inputs/outputs tested
- [ ] Coverage >80%

---

### Test-3: Write Unit Tests for Stores [T]
**Description**: Test NgRx stores and state management  
**Files**: 
- `frontend/src/app/features/trips/store/trips.store.spec.ts`
- `frontend/src/app/features/itinerary/store/itinerary.store.spec.ts`

**Tasks**:
1. Write tests for TripsStore methods
2. Write tests for ItineraryStore methods
3. Test computed signals
4. Test async operations (API calls)
5. Mock API services
6. Test error handling

**Verification**:
- [ ] All store tests pass
- [ ] State updates correctly
- [ ] Computed signals work
- [ ] API integration mocked

---

### Test-4: Write E2E Tests for Critical Flows [T]
**Description**: End-to-end testing with Cypress/Playwright  
**Files**: 
- `frontend/cypress/e2e/trip-management.cy.ts`
- `frontend/cypress/e2e/itinerary-management.cy.ts`

**Tasks**:
1. Set up Cypress or Playwright
2. Write test: Create new trip
3. Write test: Add flight to trip
4. Write test: Add transport to trip
5. Write test: Add accommodation to trip
6. Write test: View gap detection
7. Write test: Drag-drop reorder items
8. Write test: Delete trip
9. Set up test database

**Verification**:
- [ ] All E2E tests pass
- [ ] Tests run reliably
- [ ] Cover main user flows
- [ ] Test database isolated

---

### Polish-1: Implement Responsive Design [S]
**Description**: Optimize for mobile and tablet  
**Files**: 
- All component SCSS files

**Tasks**:
1. Audit all components on mobile
2. Add media queries for breakpoints
3. Adjust grid layouts for small screens
4. Make timeline vertical on mobile
5. Adjust form layouts for mobile
6. Test on various screen sizes
7. Ensure touch targets are large enough
8. Test drag-drop on touch devices

**Verification**:
- [ ] Works on mobile (320px+)
- [ ] Works on tablet (768px+)
- [ ] Works on desktop (1024px+)
- [ ] Touch interactions work
- [ ] No horizontal scroll

---

### Polish-2: Implement Accessibility Improvements [S]
**Description**: Ensure WCAG 2.1 AA compliance  
**Files**: 
- All component HTML and TS files

**Tasks**:
1. Add ARIA labels to all interactive elements
2. Ensure keyboard navigation works
3. Test with screen reader
4. Check color contrast ratios
5. Add focus indicators
6. Add skip links
7. Test tab order
8. Add alt text to icons

**Verification**:
- [ ] Keyboard navigation works throughout
- [ ] Screen reader announces correctly
- [ ] Color contrast passes WCAG AA
- [ ] Focus indicators visible
- [ ] Tab order logical

---

### Polish-3: Implement Error Handling UI [S]
**Description**: Better error messages and handling  
**Files**: 
- `frontend/src/app/core/services/notification.service.ts`
- `frontend/src/app/shared/components/error-message/`

**Tasks**:
1. Create `NotificationService` for toasts/snackbars
2. Create `ErrorMessageComponent`
3. Display errors from API calls
4. Show validation errors clearly
5. Add retry logic for failed requests
6. Show offline indicator
7. Add helpful error messages
8. Style error states

**Verification**:
- [ ] Errors displayed to user
- [ ] Messages are helpful
- [ ] Retry works
- [ ] Offline detected
- [ ] Styling consistent

---

### Polish-4: Optimize Performance [S]
**Description**: Improve load times and responsiveness  
**Files**: 
- Various files

**Tasks**:
1. Implement OnPush change detection strategy
2. Add virtual scrolling for long lists (if needed)
3. Optimize bundle size (check with webpack-bundle-analyzer)
4. Enable production mode optimizations
5. Add service worker for caching (PWA)
6. Optimize images and assets
7. Lazy load routes
8. Minimize API calls with caching

**Verification**:
- [ ] Lighthouse score >90
- [ ] Bundle size <500KB gzipped
- [ ] Load time <2 seconds
- [ ] Smooth 60fps interactions
- [ ] API calls minimized

---

### Polish-5: Add Loading States [P]
**Description**: Improve perceived performance  
**Files**: 
- All smart components

**Tasks**:
1. Show loading spinner when fetching data
2. Add skeleton loaders for trip cards
3. Add loading state to timeline
4. Show progress during saves
5. Disable forms during submission
6. Add loading indicators to buttons
7. Show loading during geocoding

**Verification**:
- [ ] Loading states visible
- [ ] User knows when waiting
- [ ] No double-submissions possible
- [ ] Skeleton loaders smooth

---

### Polish-6: Create Documentation [P]
**Description**: Document the application  
**Files**: 
- `README.md`
- `docs/DEVELOPMENT.md`
- `docs/API.md`
- `docs/DEPLOYMENT.md`

**Tasks**:
1. Update README with full project description
2. Document setup instructions
3. Document API endpoints
4. Document database schema
5. Document component architecture
6. Add code comments to complex logic
7. Create user guide
8. Document deployment process

**Verification**:
- [ ] README complete and accurate
- [ ] Setup instructions work
- [ ] API documented
- [ ] Architecture explained
- [ ] User guide helpful

---

## Summary

**Total Tasks**: 80+ tasks organized across 8 phases

**Estimated Timeline**: 7-8 weeks

**Task Distribution**:
- Phase 1 (Setup): 7 tasks
- Phase 2 (Data Layer): 5 tasks
- Phase 3 (Backend API): 8 tasks
- Phase 4 (State Management): 7 tasks
- Phase 5 (Core UI): 12 tasks
- Phase 6 (Forms & CRUD): 7 tasks
- Phase 7 (Advanced Features): 7 tasks
- Phase 8 (Testing & Polish): 11 tasks

**Key Milestones**:
1. ✅ Project foundation complete (end of Phase 1)
2. ✅ Backend API functional (end of Phase 3)
3. ✅ Basic UI operational (end of Phase 5)
4. ✅ Full CRUD working (end of Phase 6)
5. ✅ Advanced features complete (end of Phase 7)
6. ✅ Production-ready (end of Phase 8)

**Testing Approach**:
- Unit tests written alongside implementation
- Integration tests after API completion
- E2E tests before final polish
- Target: >80% code coverage

**Parallel Work Opportunities**:
Tasks marked [P] can be done simultaneously by different developers or in any order within their phase.
