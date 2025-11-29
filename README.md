# Minimal Next.js 14 Dashboard Boilerplate

This single-file boilerplate contains the minimal project structure and code snippets to get a Next.js 14 app with a simple dashboard running. It uses Tailwind CSS for styling and server/server-client components patterns. Copy the files into your project accordingly.

---

## File tree (create these files in your project)

```
package.json
next.config.js
postcss.config.js
tailwind.config.js
/app/layout.tsx
/app/page.tsx
/app/api/health/route.ts
/components/Header.tsx
/components/Sidebar.tsx
/styles/globals.css
```

---

## package.json
```json
{
  "name": "nextjs14-dashboard",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "14.0.0",
    "react": "18.2.0",
    "react-dom": "18.2.0"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "tailwindcss": "^3.4.0"
  }
}
```

---

## next.config.js
```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
}
module.exports = nextConfig
```

---

## postcss.config.js
```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

---

## tailwind.config.js
```js
module.exports = {
  content: [
    './app/**/*.{ts,tsx,js,jsx}',
    './components/**/*.{ts,tsx,js,jsx}'
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

---

## styles/globals.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

html, body, #__next {
  height: 100%;
}

body {
  @apply bg-slate-50 text-slate-900;
}
```

---

## app/layout.tsx
```tsx
import React from 'react'
import './styles/globals.css'
import Header from '../components/Header'
import Sidebar from '../components/Sidebar'

export const metadata = {
  title: 'Dashboard',
  description: 'Minimal Next.js 14 Dashboard'
}

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <div className="min-h-screen flex">
          <Sidebar />
          <div className="flex-1">
            <Header />
            <main className="p-6">{children}</main>
          </div>
        </div>
      </body>
    </html>
  )
}
```

---

## app/page.tsx
```tsx
import React from 'react'

// Server component: fetch small summary from an internal API
async function getSummary() {
  const res = await fetch('http://localhost:3000/api/health', { cache: 'no-store' })
  if (!res.ok) return { ok: false }
  return res.json()
}

export default async function Page() {
  const summary = await getSummary()

  return (
    <div className="max-w-6xl mx-auto">
      <h1 className="text-3xl font-bold mb-4">Dashboard</h1>

      <section className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
        <div className="p-4 bg-white rounded-lg shadow"> 
          <div className="text-sm text-slate-500">Status</div>
          <div className="text-xl font-semibold mt-2">{summary?.ok ? 'OK' : 'Unavailable'}</div>
        </div>
        <div className="p-4 bg-white rounded-lg shadow"> 
          <div className="text-sm text-slate-500">Active Workspaces</div>
          <div className="text-xl font-semibold mt-2">12</div>
        </div>
        <div className="p-4 bg-white rounded-lg shadow"> 
          <div className="text-sm text-slate-500">Users Online</div>
          <div className="text-xl font-semibold mt-2">34</div>
        </div>
      </section>

      <section className="bg-white rounded-lg shadow p-4">
        <h2 className="text-xl font-semibold mb-3">Recent Timesheets</h2>
        <table className="w-full text-left table-fixed">
          <thead>
            <tr className="text-sm text-slate-500">
              <th className="w-1/4 p-2">User</th>
              <th className="w-1/4 p-2">Date</th>
              <th className="w-1/4 p-2">Hours</th>
              <th className="w-1/4 p-2">Status</th>
            </tr>
          </thead>
          <tbody>
            <tr className="border-t">
              <td className="p-2">Bob Worker</td>
              <td className="p-2">2025-11-29</td>
              <td className="p-2">8.0</td>
              <td className="p-2">Pending</td>
            </tr>
            <tr className="border-t">
              <td className="p-2">Alice Admin</td>
              <td className="p-2">2025-11-28</td>
              <td className="p-2">7.5</td>
              <td className="p-2">Approved</td>
            </tr>
          </tbody>
        </table>
      </section>
    </div>
  )
}
```

---

## app/api/health/route.ts
```ts
import { NextResponse } from 'next/server'

export async function GET() {
  return NextResponse.json({ ok: true, time: new Date().toISOString() })
}
```

---

## components/Header.tsx
```tsx
'use client'
import React from 'react'

export default function Header(){
  return (
    <header className="flex items-center justify-between px-6 py-4 bg-white border-b">
      <div className="flex items-center gap-4">
        <button className="p-2 rounded-md hover:bg-slate-100">☰</button>
        <h3 className="text-lg font-semibold">Work Hours SaaS</h3>
      </div>
      <div className="flex items-center gap-4">
        <div className="text-sm text-slate-600">Admin</div>
        <div className="w-8 h-8 bg-slate-200 rounded-full"></div>
      </div>
    </header>
  )
}
```

---

## components/Sidebar.tsx
```tsx
import React from 'react'

export default function Sidebar(){
  return (
    <aside className="w-64 bg-slate-800 text-white min-h-screen p-4 hidden md:block">
      <div className="mb-8 text-xl font-bold">Dashboard</div>
      <nav className="flex flex-col gap-2 text-sm">
        <a className="p-2 rounded hover:bg-slate-700" href="#">Overview</a>
        <a className="p-2 rounded hover:bg-slate-700" href="#">Workspaces</a>
        <a className="p-2 rounded hover:bg-slate-700" href="#">Timesheets</a>
        <a className="p-2 rounded hover:bg-slate-700" href="#">Users</a>
        <a className="p-2 rounded hover:bg-slate-700" href="#">Settings</a>
      </nav>
    </aside>
  )
}
```

---

## How to run

1. Create a new folder and initialize npm: `npm init -y`
2. Copy files above into your project (create `app` and `components` folders).
3. Install dependencies: `npm install next@14 react react-dom` and dev deps `npm install -D tailwindcss postcss autoprefixer`
4. Initialize tailwind: `npx tailwindcss init -p` (or use the provided tailwind.config.js)
5. Add scripts to `package.json` (see above) and run `npm run dev`

---

This is intentionally minimal: small layout, header, sidebar, server API route. Extend this with Supabase SDK, auth, and real data fetching as next steps.
