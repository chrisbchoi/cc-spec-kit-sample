# Trip Organizer - Implementation Plan

## Technology Stack

### Frontend
- **Framework**: Angular 18+ (latest stable)
- **State Management**: NgRx with Signals
- **Styling**: SCSS with Angular Material or standalone component styles
- **Language**: TypeScript 5.0+ (strict mode)
- **Build Tool**: Angular CLI with esbuild
- **HTTP Client**: Angular HttpClient with interceptors

### Backend (BFF - Backend for Frontend)
- **Framework**: Node.js with Express or NestJS
- **Language**: TypeScript 5.0+
- **API Style**: RESTful API
- **Database**: SQLite (better-sqlite3 for Node.js)
- **ORM**: TypeORM or Kysely for type-safe queries
- **Validation**: class-validator + class-transformer

### External Services
- **Maps**: Google Maps API (server-side proxy through BFF)
- **Geocoding**: Google Geocoding API (server-side proxy through BFF)

### Development Tools
- **Linting**: ESLint with Angular rules
- **Formatting**: Prettier
- **Testing**: 
  - Jest for unit tests
  - Cypress or Playwright for E2E tests
- **Version Control**: Git with conventional commits

## Architecture Overview

### Three-Tier Architecture

```
┌─────────────────────────────────────────┐
│         Angular Frontend (SPA)          │
│  - NgRx Store with Signals              │
│  - Smart & Presentation Components      │
│  - SCSS Styling                         │
│  - Client-side validation               │
└───────────────┬─────────────────────────┘
                │ HTTP/REST
┌───────────────▼─────────────────────────┐
│      BFF API Layer (Node.js/NestJS)     │
│  - Route handlers                       │
│  - Business logic                       │
│  - API key management                   │
│  - External API proxying                │
│  - Data transformation                  │
└───────────────┬─────────────────────────┘
                │ SQL Queries
┌───────────────▼─────────────────────────┐
│         SQLite Database (Local)         │
│  - Trip data                            │
│  - Itinerary items                      │
│  - Location cache                       │
│  - User preferences                     │
└─────────────────────────────────────────┘
```

## Project Structure

```
trip-organizer/
├── frontend/                           # Angular application
│   ├── src/
│   │   ├── app/
│   │   │   ├── core/                  # Singleton services, guards, interceptors
│   │   │   │   ├── guards/
│   │   │   │   ├── interceptors/
│   │   │   │   │   └── api.interceptor.ts
│   │   │   │   ├── models/
│   │   │   │   │   ├── trip.model.ts
│   │   │   │   │   ├── itinerary-item.model.ts
│   │   │   │   │   ├── flight.model.ts
│   │   │   │   │   ├── transport.model.ts
│   │   │   │   │   ├── accommodation.model.ts
│   │   │   │   │   └── location.model.ts
│   │   │   │   ├── services/
│   │   │   │   │   ├── api.service.ts
│   │   │   │   │   ├── storage.service.ts
│   │   │   │   │   └── notification.service.ts
│   │   │   │   └── utils/
│   │   │   │       ├── date.utils.ts
│   │   │   │       ├── validation.utils.ts
│   │   │   │       └── gap-detection.utils.ts
│   │   │   ├── features/
│   │   │   │   ├── trips/
│   │   │   │   │   ├── components/
│   │   │   │   │   │   ├── trip-list/
│   │   │   │   │   │   ├── trip-form/
│   │   │   │   │   │   └── trip-card/
│   │   │   │   │   ├── pages/
│   │   │   │   │   │   ├── trips-dashboard/
│   │   │   │   │   │   ├── trip-detail/
│   │   │   │   │   │   └── trip-edit/
│   │   │   │   │   ├── store/
│   │   │   │   │   │   ├── trips.actions.ts
│   │   │   │   │   │   ├── trips.reducer.ts
│   │   │   │   │   │   ├── trips.selectors.ts
│   │   │   │   │   │   ├── trips.effects.ts
│   │   │   │   │   │   └── trips.state.ts
│   │   │   │   │   └── trips-routing.module.ts
│   │   │   │   ├── itinerary/
│   │   │   │   │   ├── components/
│   │   │   │   │   │   ├── itinerary-timeline/
│   │   │   │   │   │   ├── itinerary-item/
│   │   │   │   │   │   ├── flight-form/
│   │   │   │   │   │   ├── transport-form/
│   │   │   │   │   │   ├── accommodation-form/
│   │   │   │   │   │   ├── gap-indicator/
│   │   │   │   │   │   └── drag-drop-list/
│   │   │   │   │   ├── pages/
│   │   │   │   │   │   ├── itinerary-view/
│   │   │   │   │   │   └── add-item/
│   │   │   │   │   ├── store/
│   │   │   │   │   │   ├── itinerary.actions.ts
│   │   │   │   │   │   ├── itinerary.reducer.ts
│   │   │   │   │   │   ├── itinerary.selectors.ts
│   │   │   │   │   │   ├── itinerary.effects.ts
│   │   │   │   │   │   └── itinerary.state.ts
│   │   │   │   │   └── itinerary-routing.module.ts
│   │   │   │   └── maps/
│   │   │   │       ├── components/
│   │   │   │       │   ├── location-picker/
│   │   │   │       │   ├── location-display/
│   │   │   │       │   └── trip-map-view/
│   │   │   │       └── services/
│   │   │   │           └── maps.service.ts
│   │   │   ├── shared/
│   │   │   │   ├── components/
│   │   │   │   │   ├── date-time-picker/
│   │   │   │   │   ├── duration-display/
│   │   │   │   │   ├── location-search/
│   │   │   │   │   ├── confirmation-dialog/
│   │   │   │   │   └── loading-spinner/
│   │   │   │   ├── directives/
│   │   │   │   │   └── drag-drop.directive.ts
│   │   │   │   ├── pipes/
│   │   │   │   │   ├── duration.pipe.ts
│   │   │   │   │   ├── date-format.pipe.ts
│   │   │   │   │   └── timezone.pipe.ts
│   │   │   │   └── validators/
│   │   │   │       ├── date-range.validator.ts
│   │   │   │       └── location.validator.ts
│   │   │   ├── store/
│   │   │   │   ├── app.state.ts
│   │   │   │   └── index.ts
│   │   │   ├── app.component.ts
│   │   │   ├── app.component.scss
│   │   │   ├── app.config.ts
│   │   │   └── app.routes.ts
│   │   ├── assets/
│   │   │   ├── icons/
│   │   │   └── styles/
│   │   │       ├── _variables.scss
│   │   │       ├── _mixins.scss
│   │   │       └── _themes.scss
│   │   ├── environments/
│   │   │   ├── environment.ts
│   │   │   └── environment.prod.ts
│   │   ├── styles.scss
│   │   ├── main.ts
│   │   └── index.html
│   ├── angular.json
│   ├── tsconfig.json
│   ├── package.json
│   └── .eslintrc.json
├── backend/                            # BFF API Layer
│   ├── src/
│   │   ├── modules/
│   │   │   ├── trips/
│   │   │   │   ├── trips.controller.ts
│   │   │   │   ├── trips.service.ts
│   │   │   │   ├── trips.repository.ts
│   │   │   │   ├── dto/
│   │   │   │   │   ├── create-trip.dto.ts
│   │   │   │   │   └── update-trip.dto.ts
│   │   │   │   └── entities/
│   │   │   │       └── trip.entity.ts
│   │   │   ├── itinerary/
│   │   │   │   ├── itinerary.controller.ts
│   │   │   │   ├── itinerary.service.ts
│   │   │   │   ├── itinerary.repository.ts
│   │   │   │   ├── gap-detection.service.ts
│   │   │   │   ├── dto/
│   │   │   │   │   ├── create-flight.dto.ts
│   │   │   │   │   ├── create-transport.dto.ts
│   │   │   │   │   └── create-accommodation.dto.ts
│   │   │   │   └── entities/
│   │   │   │       ├── itinerary-item.entity.ts
│   │   │   │       ├── flight.entity.ts
│   │   │   │       ├── transport.entity.ts
│   │   │   │       └── accommodation.entity.ts
│   │   │   ├── maps/
│   │   │   │   ├── maps.controller.ts
│   │   │   │   ├── maps.service.ts
│   │   │   │   ├── geocoding.service.ts
│   │   │   │   └── location-cache.repository.ts
│   │   │   └── database/
│   │   │       ├── database.service.ts
│   │   │       ├── migrations/
│   │   │       └── data-source.ts
│   │   ├── common/
│   │   │   ├── middleware/
│   │   │   │   ├── logger.middleware.ts
│   │   │   │   └── error.middleware.ts
│   │   │   ├── filters/
│   │   │   │   └── http-exception.filter.ts
│   │   │   └── validators/
│   │   │       └── date-validator.ts
│   │   ├── config/
│   │   │   ├── app.config.ts
│   │   │   ├── database.config.ts
│   │   │   └── maps.config.ts
│   │   ├── app.module.ts
│   │   └── main.ts
│   ├── database/
│   │   └── trip-organizer.db        # SQLite database file
│   ├── tsconfig.json
│   ├── package.json
│   └── .env.example
├── shared/                             # Shared types between frontend & backend
│   └── types/
│       ├── trip.types.ts
│       ├── itinerary.types.ts
│       └── api-response.types.ts
├── .gitignore
├── README.md
└── package.json                        # Root workspace package.json
```

## Data Models

### TypeScript Interfaces (Shared)

```typescript
// Trip Model
interface Trip {
  id: string;
  title: string;
  description?: string;
  startDate: Date;
  endDate: Date;
  createdAt: Date;
  updatedAt: Date;
}

// Base Itinerary Item
interface ItineraryItemBase {
  id: string;
  tripId: string;
  type: 'flight' | 'transport' | 'accommodation';
  startDate: Date;
  endDate: Date;
  location: Location;
  notes?: string;
  confirmationNumber?: string;
  createdAt: Date;
  updatedAt: Date;
}

// Flight
interface Flight extends ItineraryItemBase {
  type: 'flight';
  flightNumber?: string;
  airline?: string;
  departureLocation: Location;
  arrivalLocation: Location;
  departureTime: Date;
  arrivalTime: Date;
  duration: number; // minutes
}

// Transportation
interface Transport extends ItineraryItemBase {
  type: 'transport';
  transportType: 'train' | 'bus' | 'car' | 'ferry' | 'taxi' | 'other';
  departureLocation: Location;
  arrivalLocation: Location;
  departureTime: Date;
  arrivalTime: Date;
  duration: number; // minutes
  provider?: string;
}

// Accommodation
interface Accommodation extends ItineraryItemBase {
  type: 'accommodation';
  name: string;
  accommodationType: 'hotel' | 'airbnb' | 'hostel' | 'home' | 'other';
  checkInTime: Date;
  checkOutTime: Date;
  address: string;
  phoneNumber?: string;
}

// Location
interface Location {
  address: string;
  city: string;
  country: string;
  latitude?: number;
  longitude?: number;
  placeId?: string; // Google Places ID
}

// Gap Detection
interface ItineraryGap {
  id: string;
  type: 'time' | 'location' | 'accommodation';
  startItem: ItineraryItemBase;
  endItem: ItineraryItemBase;
  gapDuration: number; // minutes
  severity: 'info' | 'warning' | 'error';
  message: string;
  suggestions?: string[];
}
```

### SQLite Database Schema

```sql
-- Trips Table
CREATE TABLE trips (
  id TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  description TEXT,
  start_date TEXT NOT NULL,
  end_date TEXT NOT NULL,
  created_at TEXT NOT NULL,
  updated_at TEXT NOT NULL
);

-- Itinerary Items Table (Base)
CREATE TABLE itinerary_items (
  id TEXT PRIMARY KEY,
  trip_id TEXT NOT NULL,
  type TEXT NOT NULL CHECK(type IN ('flight', 'transport', 'accommodation')),
  start_date TEXT NOT NULL,
  end_date TEXT NOT NULL,
  notes TEXT,
  confirmation_number TEXT,
  created_at TEXT NOT NULL,
  updated_at TEXT NOT NULL,
  FOREIGN KEY (trip_id) REFERENCES trips(id) ON DELETE CASCADE
);

CREATE INDEX idx_itinerary_trip_id ON itinerary_items(trip_id);
CREATE INDEX idx_itinerary_dates ON itinerary_items(start_date, end_date);

-- Flights Table
CREATE TABLE flights (
  id TEXT PRIMARY KEY,
  itinerary_item_id TEXT NOT NULL UNIQUE,
  flight_number TEXT,
  airline TEXT,
  departure_location TEXT NOT NULL, -- JSON
  arrival_location TEXT NOT NULL,   -- JSON
  departure_time TEXT NOT NULL,
  arrival_time TEXT NOT NULL,
  duration INTEGER NOT NULL,
  FOREIGN KEY (itinerary_item_id) REFERENCES itinerary_items(id) ON DELETE CASCADE
);

-- Transportation Table
CREATE TABLE transportation (
  id TEXT PRIMARY KEY,
  itinerary_item_id TEXT NOT NULL UNIQUE,
  transport_type TEXT NOT NULL,
  provider TEXT,
  departure_location TEXT NOT NULL, -- JSON
  arrival_location TEXT NOT NULL,   -- JSON
  departure_time TEXT NOT NULL,
  arrival_time TEXT NOT NULL,
  duration INTEGER NOT NULL,
  FOREIGN KEY (itinerary_item_id) REFERENCES itinerary_items(id) ON DELETE CASCADE
);

-- Accommodations Table
CREATE TABLE accommodations (
  id TEXT PRIMARY KEY,
  itinerary_item_id TEXT NOT NULL UNIQUE,
  name TEXT NOT NULL,
  accommodation_type TEXT NOT NULL,
  address TEXT NOT NULL,
  location TEXT NOT NULL, -- JSON
  check_in_time TEXT NOT NULL,
  check_out_time TEXT NOT NULL,
  phone_number TEXT,
  FOREIGN KEY (itinerary_item_id) REFERENCES itinerary_items(id) ON DELETE CASCADE
);

-- Location Cache (for geocoding results)
CREATE TABLE location_cache (
  id TEXT PRIMARY KEY,
  address TEXT NOT NULL UNIQUE,
  city TEXT,
  country TEXT,
  latitude REAL,
  longitude REAL,
  place_id TEXT,
  geocoded_at TEXT NOT NULL,
  created_at TEXT NOT NULL
);

CREATE INDEX idx_location_address ON location_cache(address);
```

## NgRx State Management

### Store Structure

```typescript
// Root State
interface AppState {
  trips: TripsState;
  itinerary: ItineraryState;
  ui: UIState;
}

// Trips Feature State
interface TripsState {
  trips: Trip[];
  selectedTripId: string | null;
  loading: boolean;
  error: string | null;
}

// Itinerary Feature State
interface ItineraryState {
  items: ItineraryItem[];
  gaps: ItineraryGap[];
  selectedItemId: string | null;
  loading: boolean;
  error: string | null;
  dragInProgress: boolean;
}

// UI State
interface UIState {
  sidenavOpen: boolean;
  theme: 'light' | 'dark';
  mapView: boolean;
}
```

### Signal-Based Approach

```typescript
// Using NgRx Signal Store (Angular 17+)
import { signalStore, withState, withMethods, withComputed } from '@ngrx/signals';

export const TripsStore = signalStore(
  { providedIn: 'root' },
  withState<TripsState>({
    trips: [],
    selectedTripId: null,
    loading: false,
    error: null
  }),
  withComputed((store) => ({
    selectedTrip: computed(() => 
      store.trips().find(t => t.id === store.selectedTripId())
    ),
    tripCount: computed(() => store.trips().length)
  })),
  withMethods((store, tripsService = inject(TripsService)) => ({
    async loadTrips() {
      patchState(store, { loading: true });
      try {
        const trips = await tripsService.getAll();
        patchState(store, { trips, loading: false });
      } catch (error) {
        patchState(store, { error: error.message, loading: false });
      }
    },
    selectTrip(id: string) {
      patchState(store, { selectedTripId: id });
    }
  }))
);
```

## API Endpoints (BFF)

### REST API Design

```typescript
// Trips
GET    /api/trips                    // Get all trips
GET    /api/trips/:id                // Get trip by ID
POST   /api/trips                    // Create new trip
PUT    /api/trips/:id                // Update trip
DELETE /api/trips/:id                // Delete trip

// Itinerary Items
GET    /api/trips/:tripId/itinerary        // Get all items for trip
GET    /api/itinerary/:id                  // Get specific item
POST   /api/trips/:tripId/itinerary/flight // Create flight
POST   /api/trips/:tripId/itinerary/transport // Create transport
POST   /api/trips/:tripId/itinerary/accommodation // Create accommodation
PUT    /api/itinerary/:id                  // Update item
DELETE /api/itinerary/:id                  // Delete item
PATCH  /api/itinerary/:id/reorder          // Reorder item (drag-drop)

// Gap Detection
GET    /api/trips/:tripId/gaps             // Get detected gaps

// Maps (Proxy to Google Maps API)
POST   /api/maps/geocode                   // Geocode address
GET    /api/maps/place/:placeId            // Get place details
POST   /api/maps/directions                // Get directions between locations

// Export
GET    /api/trips/:id/export/json          // Export as JSON
GET    /api/trips/:id/export/ical          // Export as iCalendar
GET    /api/trips/:id/export/pdf           // Export as PDF (future)
```

## Component Architecture

### Smart vs Presentational Components

**Smart Components (Container):**
- `TripsDashboardComponent` - Manages trip list
- `TripDetailComponent` - Manages single trip view
- `ItineraryViewComponent` - Manages itinerary timeline
- `AddItemComponent` - Manages item creation

**Presentational Components:**
- `TripCardComponent` - Displays trip summary
- `TripFormComponent` - Trip edit form
- `ItineraryTimelineComponent` - Visual timeline
- `ItineraryItemComponent` - Single item display
- `FlightFormComponent` - Flight input form
- `TransportFormComponent` - Transport input form
- `AccommodationFormComponent` - Accommodation input form
- `GapIndicatorComponent` - Gap warning display
- `LocationPickerComponent` - Location selection with map
- `DragDropListComponent` - Draggable item list

## Key Features Implementation

### 1. Drag-and-Drop with Angular CDK

```typescript
// Using @angular/cdk/drag-drop
import { CdkDragDrop, moveItemInArray } from '@angular/cdk/drag-drop';

drop(event: CdkDragDrop<ItineraryItem[]>) {
  const items = [...this.items()];
  moveItemInArray(items, event.previousIndex, event.currentIndex);
  
  // Update order in store and backend
  this.store.reorderItems(items);
}
```

### 2. Gap Detection Algorithm

```typescript
// gap-detection.service.ts
export class GapDetectionService {
  detectGaps(items: ItineraryItem[]): ItineraryGap[] {
    const sortedItems = [...items].sort((a, b) => 
      new Date(a.startDate).getTime() - new Date(b.startDate).getTime()
    );
    
    const gaps: ItineraryGap[] = [];
    
    for (let i = 0; i < sortedItems.length - 1; i++) {
      const current = sortedItems[i];
      const next = sortedItems[i + 1];
      
      // Time gap detection
      const timegap = this.calculateTimeGap(current, next);
      if (timegap) gaps.push(timegap);
      
      // Location mismatch detection
      const locationGap = this.checkLocationMismatch(current, next);
      if (locationGap) gaps.push(locationGap);
      
      // Missing accommodation detection
      const accommodationGap = this.checkMissingAccommodation(current, next);
      if (accommodationGap) gaps.push(accommodationGap);
    }
    
    return gaps;
  }
}
```

### 3. Google Maps Integration (through BFF)

```typescript
// Frontend: maps.service.ts
export class MapsService {
  private apiService = inject(ApiService);
  
  async geocodeAddress(address: string): Promise<Location> {
    // Call BFF endpoint, not Google directly
    return this.apiService.post<Location>('/api/maps/geocode', { address });
  }
  
  getMapUrl(location: Location): string {
    const { latitude, longitude } = location;
    return `https://www.google.com/maps/search/?api=1&query=${latitude},${longitude}`;
  }
}

// Backend: maps.service.ts
export class MapsService {
  async geocode(address: string): Promise<Location> {
    // Check cache first
    const cached = await this.locationCache.findByAddress(address);
    if (cached) return cached;
    
    // Call Google Geocoding API with server-side API key
    const result = await this.googleMapsClient.geocode({ address });
    
    // Cache result
    await this.locationCache.save(result);
    
    return result;
  }
}
```

### 4. Form Validation with Signals

```typescript
// flight-form.component.ts
export class FlightFormComponent {
  private fb = inject(FormBuilder);
  
  form = this.fb.group({
    flightNumber: [''],
    airline: [''],
    departureLocation: ['', [Validators.required]],
    arrivalLocation: ['', [Validators.required]],
    departureTime: ['', [Validators.required]],
    arrivalTime: ['', [Validators.required]]
  }, {
    validators: [dateRangeValidator('departureTime', 'arrivalTime')]
  });
  
  // Convert to signals
  departureTime = toSignal(this.form.controls.departureTime.valueChanges);
  arrivalTime = toSignal(this.form.controls.arrivalTime.valueChanges);
  
  // Computed duration
  duration = computed(() => {
    const departure = this.departureTime();
    const arrival = this.arrivalTime();
    return calculateDuration(departure, arrival);
  });
}
```

## Styling Guidelines (SCSS)

### Theme Structure

```scss
// _variables.scss
$primary-color: #1976d2;
$accent-color: #ff4081;
$warn-color: #f44336;

$flight-color: #2196f3;
$transport-color: #4caf50;
$accommodation-color: #ff9800;

$spacing-unit: 8px;
$border-radius: 4px;

// _mixins.scss
@mixin card-style {
  background: white;
  border-radius: $border-radius;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  padding: $spacing-unit * 2;
}

@mixin responsive-grid($columns: 1, $gap: $spacing-unit * 2) {
  display: grid;
  grid-template-columns: repeat($columns, 1fr);
  gap: $gap;
  
  @media (max-width: 768px) {
    grid-template-columns: 1fr;
  }
}

// Component styles
.itinerary-item {
  @include card-style;
  
  &.flight { border-left: 4px solid $flight-color; }
  &.transport { border-left: 4px solid $transport-color; }
  &.accommodation { border-left: 4px solid $accommodation-color; }
}
```

## Development Phases

### Phase 1: Foundation (Week 1-2)
1. Project setup and configuration
2. Database schema and migrations
3. Basic API endpoints (CRUD for trips)
4. Angular routing and navigation
5. NgRx store setup with signals
6. Basic trip list and detail views

### Phase 2: Core Features (Week 3-4)
1. Itinerary item models and forms
2. Flight, transport, accommodation CRUD
3. Chronological timeline view
4. Basic gap detection algorithm
5. Location input with validation
6. API integration for all features

### Phase 3: Advanced Features (Week 5-6)
1. Google Maps integration (BFF proxy)
2. Geocoding and location caching
3. Drag-and-drop reorganization
4. Enhanced gap detection
5. Visual gap indicators
6. Auto-save functionality

### Phase 4: Polish & Testing (Week 7-8)
1. Responsive design and mobile optimization
2. Accessibility improvements
3. Error handling and validation
4. Unit tests (>80% coverage)
5. E2E tests for critical flows
6. Performance optimization
7. Documentation

## Security Considerations

### BFF Layer Security
1. **API Key Protection**: Never expose Google Maps API keys to frontend
2. **Rate Limiting**: Implement rate limiting on BFF endpoints
3. **Input Validation**: Validate all inputs on both frontend and backend
4. **CORS**: Configure CORS properly for frontend-backend communication
5. **Error Handling**: Don't expose sensitive error details to frontend

### Data Security
1. **Local Storage**: SQLite database file with proper permissions
2. **No Authentication**: Single-user local app (future: add auth)
3. **Data Sanitization**: Sanitize all user inputs
4. **XSS Prevention**: Use Angular's built-in sanitization

## Performance Optimization

### Frontend
1. **Lazy Loading**: Load feature modules on demand
2. **OnPush Strategy**: Use ChangeDetectionStrategy.OnPush
3. **Virtual Scrolling**: Use CDK virtual scroll for long lists
4. **Memoization**: Use computed signals for derived state
5. **Bundle Size**: Tree-shake unused code, code splitting

### Backend
1. **Database Indexing**: Proper indexes on frequently queried columns
2. **Caching**: Cache geocoding results in database
3. **Connection Pooling**: Reuse database connections
4. **Response Compression**: Enable gzip compression
5. **Query Optimization**: Use prepared statements

## Testing Strategy

### Unit Tests
- **Models**: Test validation logic
- **Services**: Test business logic with mocks
- **Components**: Test component behavior
- **Pipes**: Test transformation logic
- **Utilities**: Test gap detection, date calculations

### Integration Tests
- **API Endpoints**: Test all CRUD operations
- **Database**: Test repository methods
- **Store**: Test actions, reducers, effects, selectors

### E2E Tests
- **User Flows**: 
  - Create trip and add items
  - Reorder items via drag-drop
  - View gaps and add items to fill
  - Export trip data

## Deployment Considerations

### Development
- Frontend: `ng serve` on port 4200
- Backend: `npm run dev` on port 3000
- Database: SQLite file in backend/database/

### Production Build
- Frontend: `ng build --configuration production`
- Backend: Compile TypeScript, bundle with dependencies
- Database: Bundle SQLite with application

### Future: Electron App
- Package as desktop app with Electron
- Include both frontend and backend
- Bundle SQLite database
- No external server needed

## Dependencies

### Frontend (package.json)
```json
{
  "dependencies": {
    "@angular/core": "^18.0.0",
    "@angular/common": "^18.0.0",
    "@angular/cdk": "^18.0.0",
    "@angular/forms": "^18.0.0",
    "@angular/router": "^18.0.0",
    "@ngrx/store": "^18.0.0",
    "@ngrx/effects": "^18.0.0",
    "@ngrx/signals": "^18.0.0",
    "rxjs": "^7.8.0",
    "tslib": "^2.6.0"
  },
  "devDependencies": {
    "@angular/cli": "^18.0.0",
    "@angular/compiler-cli": "^18.0.0",
    "typescript": "~5.4.0",
    "@typescript-eslint/eslint-plugin": "^7.0.0",
    "eslint": "^8.57.0",
    "prettier": "^3.2.0",
    "jest": "^29.7.0",
    "cypress": "^13.6.0"
  }
}
```

### Backend (package.json)
```json
{
  "dependencies": {
    "@nestjs/common": "^10.0.0",
    "@nestjs/core": "^10.0.0",
    "@nestjs/platform-express": "^10.0.0",
    "better-sqlite3": "^9.4.0",
    "typeorm": "^0.3.20",
    "class-validator": "^0.14.0",
    "class-transformer": "^0.5.1",
    "@googlemaps/google-maps-services-js": "^3.3.0",
    "dotenv": "^16.4.0",
    "express": "^4.18.0"
  },
  "devDependencies": {
    "@nestjs/cli": "^10.0.0",
    "@nestjs/testing": "^10.0.0",
    "typescript": "~5.4.0",
    "@types/node": "^20.11.0",
    "ts-node": "^10.9.0",
    "jest": "^29.7.0"
  }
}
```

## Environment Configuration

### Frontend (environment.ts)
```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:3000/api',
  mapProvider: 'google' // proxy through BFF
};
```

### Backend (.env)
```env
PORT=3000
NODE_ENV=development
DATABASE_PATH=./database/trip-organizer.db
GOOGLE_MAPS_API_KEY=your_api_key_here
ALLOWED_ORIGINS=http://localhost:4200
```

## Success Metrics

### Technical Metrics
- Test Coverage: >80%
- Build Time: <30 seconds
- Bundle Size: <500KB (gzipped)
- Lighthouse Score: >90
- API Response Time: <200ms

### User Metrics
- Time to create trip: <2 minutes
- Gap detection accuracy: >95%
- Drag-drop success rate: >99%
- Location geocoding success: >90%

## Future Enhancements

### Phase 2 Features
1. PWA support for offline mode
2. Export to PDF with styled layout
3. Multi-timezone display
4. Advanced map views with routes
5. Attachment support for tickets/documents

### Phase 3 Features
1. Cloud sync (optional backend)
2. Multi-user collaboration
3. Budget tracking
4. Weather integration
5. Mobile native apps (Ionic/Capacitor)
