# StockHive

A modern inventory management application built with Angular 17+ and Firebase. StockHive provides real-time stock tracking with audit logging, low stock alerts, and a clean, intuitive dashboard interface.

**GitHub Repository:** [Your GitHub Repo URL] (replace with your repository URL after creating it)
**Live Demo:** https://stockhive-9ef91.web.app

## Features

- **Real-time Product Management**: Live-updating product list with Firebase Firestore
- **Stock Adjustment**: Quick +/- buttons for stock in/out with transaction-based audit logging
- **Low Stock Alerts**: Visual highlighting when quantity falls below reorder level
- **Search & Filter**: Client-side search by name/SKU and low stock toggle
- **Edit & Delete**: Full CRUD operations with confirmation dialogs
- **Secure Authentication**: Firebase Auth with protected routes
- **Reactive Forms**: Angular Reactive Forms with comprehensive validation
- **TypeScript Strict Mode**: Full type safety with strict null checks
- **Modern Architecture**: Standalone components, signals, and inject() for DI

## Tech Stack

- **Angular 17+** with standalone components
- **Firebase** (Firestore + Auth)
- **Angular Signals** for reactive state management
- **Reactive Forms** for form handling
- **SCSS** for styling
- **TypeScript** with strict mode enabled

## Setup Instructions

### Prerequisites

- Node.js (v18 or higher)
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd stockhive
```

2. Install dependencies:
```bash
npm install
```

3. Configure Firebase:

Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com) and enable:
- Authentication (Email/Password)
- Firestore Database

Update the Firebase configuration in `src/environments/environment.ts`:

```typescript
export const environment = {
  production: false,
  firebase: {
    apiKey: 'YOUR_API_KEY',
    authDomain: 'YOUR_PROJECT_ID.firebaseapp.com',
    projectId: 'YOUR_PROJECT_ID',
    storageBucket: 'YOUR_PROJECT_ID.appspot.com',
    messagingSenderId: 'YOUR_MESSAGING_SENDER_ID',
    appId: 'YOUR_APP_ID'
  }
};
```

4. Deploy Firestore Security Rules:

Use the provided `firestore.rules` file in your Firebase Console under Firestore > Rules.

### Development Server

Start the development server:
```bash
ng serve
```

Navigate to `http://localhost:4200/`. The application will automatically reload on file changes.

### Build for Production

Build the project:
```bash
ng build
```

The build artifacts will be stored in the `dist/` directory.

## Demo Login Credentials

A demo user has already been created for testing — use the credentials below to sign in:

- **Email**: `demo@stockhive.com`
- **Password**: `demo123`

## Usage Guide

### Dashboard

The dashboard is the main interface for managing your inventory:

#### Adding Products

1. Click the "Add Product" button in the top-right
2. Fill in the form:
   - **Product Name**: Required
   - **SKU**: Required (Stock Keeping Unit)
   - **Quantity**: Required, minimum 0
   - **Reorder Level**: Required, minimum 0 (threshold for low stock alerts)
3. Click "Add Product" to save

#### Viewing Products

Products are displayed in a table with:
- **Name**: Product name
- **SKU**: Stock Keeping Unit
- **Quantity**: Current stock level
- **Reorder Level**: Threshold for low stock alerts
- **Actions**: Stock adjustment, edit, and delete buttons

#### Adjusting Stock

Use the +/- buttons in the Actions column:
- **+ (Green)**: Increase stock by 1
- **- (Red)**: Decrease stock by 1

Each adjustment is logged in the movements collection with:
- Product ID
- Delta (change amount)
- Timestamp
- User ID who made the change

*Note: Stock cannot go below 0. The transaction will fail if this would happen.*

#### Low Stock Alerts

Rows where `quantity <= reorderLevel` are highlighted in yellow with red quantity text. Use the "Show low stock only" toggle to filter for products that need reordering.

#### Searching

Use the search input to filter products by:
- Product name (case-insensitive)
- SKU (case-insensitive)

#### Editing Products

1. Click the edit icon (✎) in the Actions column
2. The form opens in a modal pre-filled with current data
3. Make your changes
4. Click "Update Product" to save

#### Deleting Products

1. Click the delete icon (✕) in the Actions column
2. Confirm the deletion in the dialog
3. The product is permanently removed

### Authentication

- **Login**: Navigate to `/login` and enter your credentials
- **Logout**: Click the "Logout" button in the dashboard header
- **Protected Routes**: The dashboard and product form require authentication

## Project Structure

```
src/
├── app/
│   ├── core/
│   │   ├── guards/          # Route guards (auth)
│   │   ├── models/          # TypeScript interfaces
│   │   └── services/        # Angular services (auth, product)
│   ├── features/
│   │   ├── dashboard/       # Main dashboard component
│   │   ├── login/           # Login component
│   │   └── product-form/    # Reusable product form component
│   ├── app.config.ts        # Firebase configuration
│   └── app.routes.ts        # Route definitions
├── environments/
│   ├── environment.ts       # Development environment config
│   └── environment.prod.ts  # Production environment config
```

## Firestore Security Rules

The application uses the following security rules (see `firestore.rules`):

- **Products**: Authenticated users can read and write
- **Movements**: Authenticated users can read and create, but updates and deletes are blocked to ensure audit log integrity

## Development Notes

- TypeScript strict mode is enabled for type safety
- All observables use `take(1)` or async pipe to prevent memory leaks
- Components use `inject()` for dependency injection instead of constructor injection
- Signals are used for local component state
- Reactive Forms are used for all form handling with comprehensive validation

## License

This project is provided as-is for educational and commercial use.

