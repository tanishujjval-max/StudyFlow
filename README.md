# StudyFlow
Plan your assignments, optimize your study time, and improve your notes with AI — all in one sleek, distraction-free platform.
/studyflow
  /pages
    index.js
    api/schedule.js
    api/improveText.js
  /components
    AssignmentForm.js
    ScheduleDisplay.js
    FocusTimer.js
  /styles
    globals.css
  next.config.js
  package.json
import Head from 'next/head';
import AssignmentForm from '../components/AssignmentForm';
import ScheduleDisplay from '../components/ScheduleDisplay';
import { useState } from 'react';

export default function Home() {
  const [schedule, setSchedule] = useState([]);

  return (
    <div className="min-h-screen bg-gray-50 dark:bg-gray-900 text-gray-900 dark:text-gray-100">
      <Head>
        <title>StudyFlow - Focus Smarter. Study Better.</title>
      </Head>

      <header className="p-6 text-center">
        <h1 className="text-4xl font-bold mb-2">StudyFlow</h1>
        <p className="text-lg">Focus smarter. Study better.</p>
      </header>

      <main className="max-w-3xl mx-auto p-4">
        <AssignmentForm setSchedule={setSchedule} />
        <ScheduleDisplay schedule={schedule} />
      </main>
    </div>
  );
}
import { useState } from 'react';

export default function AssignmentForm({ setSchedule }) {
  const [title, setTitle] = useState('');
  const [dueDate, setDueDate] = useState('');

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!title || !dueDate) return;

    // Call API to generate schedule
    const res = await fetch('/api/schedule', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify([{ title, dueDate }])
    });
    const data = await res.json();
    setSchedule(data.schedule);
    setTitle('');
    setDueDate('');
  };

  return (
    <form onSubmit={handleSubmit} className="mb-6">
      <input
        type="text"
        placeholder="Assignment Title"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        className="p-2 border rounded mr-2 w-2/3"
      />
      <input
        type="date"
        value={dueDate}
        onChange={(e) => setDueDate(e.target.value)}
        className="p-2 border rounded mr-2"
      />
      <button type="submit" className="bg-indigo-500 text-white px-4 py-2 rounded">
        Generate Schedule
      </button>
    </form>
  );
}
export default function ScheduleDisplay({ schedule }) {
  if (!schedule || schedule.length === 0) return null;

  return (
    <div className="bg-white dark:bg-gray-800 p-4 rounded shadow">
      <h2 className="text-2xl font-semibold mb-2">Your Study Schedule</h2>
      <ul>
        {schedule.map((item, idx) => (
          <li key={idx} className="border-b py-2">
            <span className="font-bold">{item.date}:</span> {item.assignment}
          </li>
        ))}
      </ul>
    </div>
  );
}
export default async function handler(req, res) {
  if (req.method === 'POST') {
    const assignments = req.body; // [{title, dueDate}]

    // Placeholder: Generate dummy schedule
    const schedule = assignments.map((a) => ({
      date: a.dueDate,
      assignment: a.title + ' - Study block 1'
    }));

    // Later replace with OpenAI API call
    res.status(200).json({ schedule });
  } else {
    res.status(405).json({ error: 'Method not allowed' });
  }
}
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  transition: background-color 0.3s, color 0.3s;
}
{
  "name": "studyflow",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "13.5.2",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "tailwindcss": "^3.4.0"
  }
}
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
}

module.exports = nextConfig;
