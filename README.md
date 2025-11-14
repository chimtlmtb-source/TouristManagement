# Tourist Management Android Application - Code Documentation

## Table of Contents
1. [Application Overview](#application-overview)
2. [Architecture](#architecture)
3. [Project Structure](#project-structure)
4. [Main Components](#main-components)
5. [Code Flow](#code-flow)
6. [User Interface](#user-interface)
7. [Database Structure](#database-structure)
8. [Authentication System](#authentication-system)
9. [Usage Instructions](#usage-instructions)
10. [Development Setup](#development-setup)

## Application Overview

The Tourist Management Application is an Android app designed to help tourists explore Vietnam. It provides information about tourist locations, local specialties, essential communication phrases, and emergency contacts.

### Key Features
- **Tourist Locations**: Browse and explore Vietnamese tourist destinations
- **Local Specialties**: Discover local food and cultural specialties
- **Communication Phrases**: Learn essential Vietnamese phrases with pronunciation
- **Emergency Contacts**: Quick access to important phone numbers
- **Admin Panel**: Content management system for administrators
- **User Profiles**: Personal account management

## Architecture

The application follows **MVVM (Model-View-ViewModel)** architecture pattern with the following layers:

```
┌─────────────────┐
│   Presentation  │ ← Activities, Fragments, Adapters
├─────────────────┤
│   ViewModel     │ ← Business Logic, LiveData
├─────────────────┤
│   Repository    │ ← Data Layer Abstraction
├─────────────────┤
│   Database      │ ← Room Database, DAOs
└─────────────────┘
```

### Technology Stack
- **Language**: Java
- **Database**: Room (SQLite)
- **UI**: Data Binding, Material Design
- **Architecture**: MVVM + Repository Pattern
- **Async**: Executor Service for background operations

## Project Structure

```
app/src/main/java/com/example/touristmanagement/
├── data/
│   ├── database/
│   │   ├── entity/          # Database entities
│   │   ├── dao/             # Data Access Objects
│   │   └── TouristDatabase  # Room database
│   ├── repository/          # Repository interfaces/implementations
│   └── sample/              # Sample data loaders
├── domain/
│   └── usecase/             # Business logic use cases
├── presentation/
│   ├── ui/
│   │   ├── auth/            # Authentication screens
│   │   ├── main/            # Main app screens
│   │   ├── admin/           # Admin panel
│   │   ├── detail/          # Detail screens
│   │   └── profile/         # User profile
│   ├── viewmodel/           # ViewModels
│   └── adapter/             # RecyclerView adapters
└── utils/                   # Utility classes
```

## Main Components

### 1. Authentication System
- **LoginActivity**: User authentication
- **RegisterActivity**: New user registration
- **AuthViewModel**: Handles authentication logic
- **SessionManager**: Manages user session

### 2. Main Application (User Interface)
- **DashboardActivity**: Main container with bottom navigation
- **LocationsFragment**: Tourist locations listing
- **SpecialtiesFragment**: Local specialties with search/filter
- **PhrasesFragment**: Communication phrases
- **EmergencyFragment**: Emergency contacts
- **UserProfileFragment**: User account management

### 3. Admin Panel
- **AdminActivity**: Admin dashboard with tabs
- **AdminDashboardFragment**: Statistics overview
- **AdminUserManagementFragment**: User management
- **AdminLocationManagementFragment**: Location CRUD operations
- **AdminSpecialtyManagementFragment**: Specialty management
- **AdminPhrasesManagementFragment**: Phrase management

### 4. Detail Screens
- **LocationDetailActivity**: Location information and specialties
- **SpecialtyDetailActivity**: Detailed specialty information
- **PhraseDetailActivity**: Phrase details with pronunciation

## Code Flow

### 1. Application Startup Flow

```
MainActivity
├── Check SessionManager.isLoggedIn()
├── If logged in:
│   ├── Check SessionManager.isAdmin()
│   ├── If admin → AdminActivity
│   └── If user → DashboardActivity
└── If not logged in → LoginActivity
```

### 2. User Authentication Flow

```
LoginActivity
├── Enter credentials
├── AuthViewModel.login()
├── Repository validates user
├── If successful:
│   ├── SessionManager.saveSession()
│   ├── Check user role
│   └── Navigate to appropriate dashboard
└── If failed → Show error message
```

### 3. Main Navigation Flow

```
DashboardActivity (Bottom Navigation)
├── Locations → LocationsFragment
├── Specialties → SpecialtiesFragment
├── Phrases → PhrasesFragment
├── Emergency → EmergencyFragment
└── Profile → UserProfileFragment
```

### 4. Data Flow Pattern

```
Fragment/Activity
├── Observes ViewModel LiveData
├── ViewModel calls Repository
├── Repository queries Database (DAO)
├── Database returns data
├── Repository processes data
├── ViewModel updates LiveData
└── UI updates automatically
```

## User Interface

### Main User Screens

1. **Locations Screen**
   - Grid/List of tourist locations
   - Search functionality
   - Click to view details

2. **Specialties Screen**
   - List of local specialties
   - Search and filter options
   - Category filtering

3. **Phrases Screen**
   - Communication phrases
   - Search by category
   - Favorite functionality
   - Usage tracking

4. **Emergency Screen**
   - Emergency contact numbers
   - Direct call functionality

5. **Profile Screen**
   - User information
   - Edit profile option
   - About and logout

### Admin Screens

1. **Dashboard Tab**
   - Statistics cards (Users, Locations, Specialties, Phrases)
   - Overview metrics

2. **Users Tab**
   - User list with search
   - Add/Edit/Delete users
   - Role management
   - Account activation/deactivation

3. **Locations Tab**
   - Location management
   - Add/Edit/Delete locations
   - View location details

4. **Phrases Tab**
   - Phrase management
   - Usage frequency tracking
   - Category management

5. **Specialties Tab**
   - Specialty management
   - Image URL management
   - Category filtering

## Database Structure

### Core Entities

1. **UserEntity**
   ```java
   - userId (PK)
   - username
   - passwordHash
   - email
   - fullName
   - country
   - preferredLanguage
   - userRole (USER/ADMIN)
   - isActive
   - createdAt
   - lastLogin
   ```

2. **LocationEntity**
   ```java
   - id (PK)
   - name
   - description
   - address
   - latitude/longitude
   - category
   - openingHours
   - entranceFee
   - imageUrls
   - historicalInfo
   - culturalInfo
   ```

3. **SpecialtyEntity**
   ```java
   - id (PK)
   - name
   - description
   - ingredients
   - preparationMethod
   - culturalSignificance
   - imageUrls
   - locationId (FK)
   ```

4. **PhraseEntity**
   ```java
   - id (PK)
   - vietnameseText
   - englishText
   - phonetic
   - category
   - difficultyLevel
   - usageFrequency
   - isFavorite
   ```

## Authentication System

### Session Management
- **SessionManager**: Handles user session persistence
- **AuthenticationManager**: Manages authentication state
- **Role-based Access**: Different interfaces for users and admins

### Security Features
- Password hashing
- Session timeout
- Role-based permissions
- Admin access restrictions

## Usage Instructions

### For End Users

1. **Getting Started**
   - Launch app
   - Register new account or login
   - Navigate using bottom navigation

2. **Exploring Locations**
   - Browse locations in Locations tab
   - Use search to find specific places
   - Tap location for detailed information

3. **Learning Phrases**
   - Access Phrases tab
   - Search by category or keyword
   - Mark phrases as favorites
   - Practice pronunciation

4. **Emergency Contacts**
   - Access Emergency tab
   - Tap phone numbers to call directly

### For Administrators

1. **Admin Access**
   - Login with admin credentials
   - Access admin panel automatically

2. **Managing Content**
   - Use tabs to navigate different management areas
   - Add/Edit/Delete content using floating action buttons
   - Use search and filters to find specific items

3. **User Management**
   - View user statistics
   - Manage user accounts
   - Monitor user activity

## Development Setup

### Prerequisites
- Android Studio
- Java 8+
- Android SDK (API 24+)

### Building the Project
1. Clone the repository
2. Open in Android Studio
3. Sync project with Gradle files
4. Run on emulator or device

### Database Initialization
- Sample data is loaded automatically on first run
- Database schema is created by Room migration

### Adding New Features

1. **Adding New Entity**
   - Create entity class in `data/database/entity/`
   - Add DAO interface in `data/database/dao/`
   - Update database version and migration

2. **Adding New Screen**
   - Create Activity/Fragment in appropriate package
   - Create ViewModel with Factory
   - Add navigation logic
   - Update layouts and strings

3. **Admin Features**
   - Add new tab in AdminPagerAdapter
   - Create management fragment
   - Implement CRUD operations
   - Add appropriate permissions

### Code Style Guidelines
- Follow MVVM architecture pattern
- Use Data Binding for UI
- Implement proper error handling
- Add null safety checks
- Comment complex business logic

## Performance Considerations

- **Database Queries**: Use efficient queries with proper indexing
- **Memory Management**: Properly handle fragment lifecycle
- **Background Operations**: Use ExecutorService for database operations
- **Image Loading**: Optimize image URLs and caching
- **Search Operations**: Implement debounced search for better UX


