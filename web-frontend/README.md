# HMS Web Dashboard (Angular)

Angular-based admin dashboard for hospital staff with role-based access control.

## Overview

Enterprise-grade Angular web application for hospital staff (Owner, Admin, Doctor, Receptionist) with comprehensive patient management, appointment scheduling, and medical records system.

## Features

### Authentication & Authorization

- JWT-based authentication with refresh token rotation
- Role-based UI — Different dashboards per role
- Route guards — Prevent unauthorized access
- HTTP interceptors — Auto-attach JWT, handle 401/403

### Employee Management (Admin/Owner)

- Create, approve, reject, update, delete employees
- Approval workflow for new registrations
- Profile change request system
- Audit log viewer

### Patient Management

- Register new patients with UHID generation
- Search patients by name, UHID, phone
- Update patient information
- View patient history

### Appointment Scheduling

- Book appointments with double-booking prevention
- View available doctor slots
- Cancel/reschedule appointments
- Mark appointments as completed/unattended
- Email notifications

### Medical Records

- Create draft medical records
- Verify and finalize records (Doctor/Admin)
- Link records to appointments
- Draft → Verified → Finalized workflow

### Dashboard Analytics

- Role-specific statistics
- Appointment trends
- Patient demographics
- Employee performance metrics

### System Administration (Owner)

- Manage permissions per role
- Configure sidebar navigation nodes
- System-wide settings

## Architecture

```text
Angular Application
    │
    ├── Core (Services, Guards, Interceptors)
    ├── Features (Lazy-loaded modules)
    │   ├── Auth
    │   ├── Dashboard
    │   ├── Patients
    │   ├── Appointments
    │   ├── Employees
    │   ├── Medical Records
    │   └── Settings
    └── Shared (Reusable components, pipes)
```

## Folder Structure

```text
web-frontend/
├── src/
│   ├── app/
│   │   ├── core/
│   │   │   ├── guards/         # Route guards
│   │   │   ├── interceptors/   # HTTP interceptors
│   │   │   ├── models/         # TypeScript interfaces
│   │   │   ├── services/       # API services
│   │   │   └── validators/     # Form validators
│   │   ├── features/
│   │   │   ├── auth/           # Login, register, forgot password
│   │   │   ├── dashboard/      # Role-specific dashboards
│   │   │   └── home/           # Main layout
│   │   └── shared/
│   │       ├── pipes/          # Custom pipes
│   │       └── ui/             # Reusable components
│   ├── assets/                 # Images, fonts, icons
│   ├── environments/           # Environment configs
│   └── styles/                 # Global styles
├── angular.json
├── package.json
└── README.md
```

## Quick Start

### Prerequisites

- Node.js 14+
- Angular CLI 14+
- npm or yarn

### Installation

```bash
# Install Angular CLI globally
npm install -g @angular/cli

# Install dependencies
npm install
```

### Environment Configuration

Update `src/environments/environment.ts`:

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:5000/api'
};
```

Update `src/environments/environment.prod.ts` for production:

```typescript
export const environment = {
  production: true,
  apiUrl: 'https://your-api-domain.com/api'
};
```

### Run Development Server

```bash
npm start

# or

ng serve
```

Navigate to:

```text
http://localhost:4200/
```

### Build for Production

```bash
npm run build

# or

ng build --configuration production
```

Output will be generated in the `dist/` folder.

## Default Login Credentials

| Role | Email | Password |
|--------|--------|--------|
| Owner | owner@hospital.com | Owner@123 |
| Admin | admin@hospital.com | Admin@123 |
| Doctor | doctor@hospital.com | Doctor@123 |
| Receptionist | receptionist@hospital.com | Receptionist@123 |

## UI Features

### Layout

- Responsive design — Mobile, tablet, desktop
- Sidebar navigation — Role-based menu visibility
- Top navbar — User profile, notifications, logout
- Breadcrumbs — Navigation tracking

### Components

- Data tables — Sortable, filterable, paginated
- Forms — Reactive forms with validation
- Modals — Create/edit dialogs
- Toast notifications — Success/error messages
- Loading states — Spinners, skeletons
- Confirmation dialogs — Delete confirmations

### Styling

- Angular Material — Material Design components
- Custom theme — Hospital-friendly color palette
- Responsive grid — Bootstrap-like layout
- Material Icons

## Security Features

### Route Guards

```typescript
// auth.guard.ts - Protects authenticated routes
// role.guard.ts - Restricts by role
// permission.guard.ts - Checks specific permissions
```

### HTTP Interceptors

```typescript
// auth.interceptor.ts - Attach JWT to requests
// error.interceptor.ts - Handle 401/403/500 globally
```

### Token Management

- Access token stored in localStorage
- Refresh token stored in httpOnly cookie (backend)
- Auto-refresh on 401 errors
- Auto-logout on token expiry

## State Management

- Services — Singleton services for shared state
- BehaviorSubjects — Reactive state updates
- LocalStorage — Persist user session

## Testing

```bash
# Run unit tests
npm test

# or

ng test

# Run e2e tests
npm run e2e

# or

ng e2e

# Code coverage
ng test --code-coverage
```

## Deployment

### Deploy to Vercel

```bash
npm i -g vercel

npm run build

vercel --prod
```

### Deploy to Netlify

```bash
npm run build
```

Upload the generated `dist/` folder or connect the GitHub repository.

### Deploy to Firebase Hosting

```bash
npm i -g firebase-tools

firebase login

firebase init hosting

firebase deploy
```

## Performance Optimizations

- Lazy loading — Feature modules loaded on demand
- OnPush change detection — Reduce unnecessary checks
- TrackBy functions — Optimize *ngFor rendering
- Production build — AOT compilation and tree-shaking
- Bundle size — ~500KB gzipped

## Tech Stack

- Framework: Angular 14+
- Language: TypeScript
- UI Library: Angular Material
- HTTP Client: Angular HttpClient
- Forms: Reactive Forms
- Routing: Angular Router
- Icons: Material Icons
- Build Tool: Angular CLI + Webpack

## Scripts

```bash
npm start              # Start dev server
npm run build          # Production build
npm test               # Run tests
npm run lint           # Lint code
npm run format         # Format code
ng generate component  # Generate component
ng generate service    # Generate service
```

## Configuration

### angular.json

Key configurations:

- Output path: `dist/web-frontend`
- Styles: `src/styles.scss`
- Assets: `src/assets`
- Production optimization: enabled

### tsconfig.json

Path aliases configured:

```json
{
  "paths": {
    "@core/*": ["src/app/core/*"],
    "@shared/*": ["src/app/shared/*"],
    "@features/*": ["src/app/features/*"]
  }
}
```

## Troubleshooting

### Port Already in Use

```bash
ng serve --port 4201
```

### CORS Errors

```text
Backend must allow http://localhost:4200 in CORS.
Check backend CORS configuration.
```

### API Connection Failed

```text
Verify backend is running on localhost:5000.
Check environment.ts apiUrl is correct.
```

### Build Errors

```bash
rm -rf node_modules package-lock.json

npm install

rm -rf .angular
```

## Responsive Breakpoints

```scss
// Mobile
@media (max-width: 767px) { }

// Tablet
@media (min-width: 768px) and (max-width: 1023px) { }

// Desktop
@media (min-width: 1024px) { }
```

## Future Enhancements

- Progressive Web App (PWA)
- Offline mode with service workers
- Real-time notifications (WebSocket)
- Advanced analytics dashboards
- Multi-language support (i18n)
- Dark mode theme

## Development Guidelines

### Component Structure

```typescript
@Component({
  selector: 'app-component-name',
  templateUrl: './component-name.component.html',
  styleUrls: ['./component-name.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ComponentNameComponent implements OnInit {
  // Use OnPush for performance
}
```

### Service Pattern

```typescript
@Injectable({ providedIn: 'root' })
export class DataService {
  private apiUrl = environment.apiUrl;

  constructor(private http: HttpClient) {}

  getData(): Observable<any> {
    return this.http.get(`${this.apiUrl}/endpoint`);
  }
}
```

## Documentation

- ../README.md
- Architecture Overview
- ../API_DOCUMENTATION.md
- ../backend/DATABASE_SCHEMA.md
- ../backend/README.md

## License

MIT

## Author

**Varshith Jakkula**

GitHub:  
https://github.com/Varshith6690

Portfolio:  
https://varshith-portfolio-self.vercel.app/

LinkedIn:  
https://www.linkedin.com/in/varshith-jakkula-34145a273/

Email:  
21r21a6690@gmail.com