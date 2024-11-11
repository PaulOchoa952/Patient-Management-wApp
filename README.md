# Patient Management System

A Next.js application for managing patient records using Supabase as the backend.

## Features

- 🔐 Authentication with Supabase Auth
- 👥 Patient Management (CRUD operations)
- 📅 Appointment Scheduling
- 🔒 Secure Vault for Sensitive Data
- 🌙 Dark Mode UI
- 🚀 Real-time Updates
- 📱 Responsive Design

## Tech Stack

- **Frontend Framework:** Next.js 14 with TypeScript
- **Styling:** Tailwind CSS
- **Database:** Supabase
- **Authentication:** Supabase Auth
- **State Management:** React Context
- **UI Components:** Custom components with Tailwind
- **Form Handling:** React Hook Form
- **Notifications:** React-Toastify

## Prerequisites

- Node.js 18.17 or later
- npm or yarn
- Supabase account and project

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd patient-management-system
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
Create a `.env.local` file in the root directory:
```bash
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

## Database Setup

1. Start Supabase locally with Docker:
```bash
npx supabase start
```

2. Create the database tables in Supabase:

```sql
-- Patients table
create table patients (
  patient_id bigint generated by default as identity primary key,
  first_name text not null,
  last_name text not null,
  date_of_birth date not null,
  gender text,
  contact_number text,
  email text,
  address text,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- Appointments table
create table appointments (
  appointment_id bigint generated by default as identity primary key,
  patient_id bigint references patients(patient_id),
  doctor_name text not null,
  appointment_date date not null,
  appointment_time time not null,
  reason text,
  status text check (status in ('Scheduled', 'Completed', 'Cancelled')) default 'Scheduled',
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);
--Access logs table
CREATE TABLE access_logs (
  id SERIAL PRIMARY KEY,
  patient_id INTEGER REFERENCES patients(patient_id), -- Assuming you have a patients table
  action TEXT NOT NULL,
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  api_key_used TEXT NOT NULL
);

-- Indexes
create index idx_appointments_patient_id on appointments(patient_id);
create index idx_appointments_date on appointments(appointment_date);
```

## Development Tools Setup

1. **Plop (Code Generator)**
```bash
npm install --save-dev plop
```

2. **Tailwind CSS**
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

3. **ESLint & Prettier**
```bash
npm install -D eslint prettier eslint-config-prettier eslint-plugin-prettier
```

4. **Supabase Client**
```bash
npm install @supabase/supabase-js
```

5. **Supabase CLI (for local development)**
```bash
npm install --save-dev supabase
```

## Available Scripts

```bash
# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Generate new component
npm run plop component ComponentName

# Generate new service
npm run plop service ServiceName

# Start local Supabase
npx supabase start

# Stop local Supabase
npx supabase stop
```

## Project Structure

```
src/
├── app/                    # Next.js app router pages
├── components/            # Reusable components
│   └── PatientForm/      # Patient form component
├── services/             # API services
│   └── patientService.ts # Patient CRUD operations
├── types/                # TypeScript types/interfaces
│   └── patient.ts       # Patient type definitions
├── lib/                  # Third-party library configurations
│   └── supabaseClient.ts # Supabase client configuration
└── styles/               # Global styles
```

## Type Definitions

The application uses TypeScript interfaces for type safety:

```typescript
interface Patient {
  patient_id: number;
  first_name: string;
  last_name: string;
  date_of_birth: string;
  gender: string;
  contact_number: string;
  email: string;
  address: string;
  created_at?: string;
}
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Troubleshooting

### Common Issues

1. **Supabase Connection Issues**
   - Verify your environment variables are correctly set
   - Ensure Supabase Docker container is running
   - Check network connectivity

2. **TypeScript Errors**
   - Run `npm run type-check` to verify types
   - Ensure all required fields are included in interfaces

3. **Database Errors**
   - Verify table schema matches type definitions
   - Check Supabase policies are correctly set

## License

This project is licensed under the MIT License - see the LICENSE file for details


