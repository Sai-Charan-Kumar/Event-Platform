# Event-Platform

This is a comprehensive platform for managing college events. This application provides a full-featured system for **Organizers** to create venues and events, and for **Attendees** to register for, book, and manage their tickets.

The system includes a Node.js (Express) backend, a SQLite database, and a JavaScript front-end.

## Features

### General

  * **Secure Authentication:** User registration with **email OTP verification** and login.
  * **Role-Based Access:** Clear separation between Organizer and Attendee roles.
  * **Dark Mode:** A persistent dark mode toggle for user preference.

### Organizer Features

  * **Venue Management:** Create and delete event venues.
  * **Event Management:** Create, edit, and delete events.
  * **Advanced Validation:** Server-side checks prevent event time conflicts, ensure events are in the future, and manage event duration (15 min min, 8-hour max) with a 2-hour buffer between events at the same venue.
  * **Admin Dashboard:** View all created events, with past events clearly marked.
  * **Attendee Tracking:** View a live list of attendees for each event.
  * **Event Check-In:** A dedicated page to check in attendees using their unique ticket ID.
  * **Post-Event Reporting:** Generate a "View Report" for finished events to see stats on revenue, tickets sold, attendance rate, and sell-through rate.

### Attendee Features

  * **Event Dashboard:** View and filter all available events (All, Active, Inactive).
  * **Ticket Booking:** Book multiple tickets at once (up to 4 per event).
  * **"My Tickets" Page:** A personal page to view all acquired tickets and their status (purchased, checked-in).
  * **Ticket Cancellation:** Ability to cancel a ticket up to 30 minutes before the event starts for a **90% refund** (10% cancellation fee).
  * **Email Reminders:** Automatically receives an email reminder 30 minutes before each event's start time.

-----

## Prerequisites

Before you begin, you will need the following installed on your machine:

  * [Node.js](https://nodejs.org/en) (v18 or later is recommended)
  * [npm](https://www.npmjs.com/) (This is included with Node.js)
  * A **Gmail Account** to send OTP and reminder emails.

-----

## Installation & Setup

Follow these steps to get the project running on your local machine.

### 1\. Download the Project

Download or clone all the project files (`server.js`, `package.json`, `.html`, `.css`, `.js`) into a new directory.
college-event-platform/
│
├── public/                  # Frontend files
│   ├── index.html          # Login/Register page
│   ├── dashboard.html      # Student dashboard
│   ├── admin.html          # Admin dashboard
│   ├── mytickets.html      # Student tickets page
│   ├── attendees.html      # Admin attendees page
│   ├── script.js           # Frontend JavaScript
│   └── style.css           # Styles with dark mode
│
├── server.js               # Main server file
├── package.json            # Dependencies
├── .env                    # Configuration (create this)
├── events.db               # SQLite database (auto-created)

### 2\. Install Dependencies

Open a terminal in the project's root directory and run:

```bash
npm install
```

This will install all the required packages from `package.json`, including:

  * `express`
  * `sqlite3`
  * `bcrypt`
  * `express-session`
  * `dotenv`
  * `nodemailer`
  * `node-cron`

### 3\. Set Up Your Environment (.env)

This is the most important step. The server relies on a `.env` file for database, session, and email credentials.

1.  Create a new file named `.env` in the root of your project directory.

2.  Copy and paste the template below into your new `.env` file:

    ```env
    # Email Configuration for Reminders (Use a Gmail App Password, not your regular password)
    EMAIL_USER=your-gmail-address@gmail.com
    EMAIL_APP_PASSWORD=your-16-digit-app-password

    # Session Secret (Generate a new one)
    SESSION_SECRET=your-own-random-32-character-secret-key

    # Server Configuration
    PORT=3000
    ```

### 4\. Get a Gmail "App Password" (for `EMAIL_APP_PASSWORD`)

You cannot use your regular Gmail password. You must generate a 16-digit "App Password" from your Google Account settings.

1.  Go to your Google Account settings: [myaccount.google.com](https://myaccount.google.com/)
2.  Go to **Security**.
3.  Ensure **2-Step Verification** is turned **ON**. You cannot get an App Password without this.
4.  Under "Signing in to Google," click on **App Passwords**.
5.  For "Select app," choose **Mail**.
6.  For "Select device," choose **Other (Custom name)** and give it a name like "Node Event App".
7.  Google will generate a 16-digit password. [cite\_start]**Use this password** (without spaces) for the `EMAIL_APP_PASSWORD` value in your `.env` file. [cite: 1]

### 5\. Generate a Session Secret

Replace `your-own-random-32-character-secret-key` with a long, random string. You can use an [online generator](https://www.random.org/strings/) for this.

-----

## Running the Application

Once you have installed the dependencies and configured your `.env` file, you can start the server.

```bash
npm start
```

The server will start, and you should see the following in your terminal:

```
Connected to the events.db SQLite database.
✓ Email service configured successfully
✓ Email reminder system initialized...
Server running on http://localhost:3000
```

> **Note:** The `events.db` database file will be automatically created in your project directory the first time you run the server.

-----

## How to Use the Platform

### 1\. Register Accounts

1.  Open your browser and go to **`http://localhost:3000`**.
2.  Click "Register" and create two accounts:
      * One with the **Admin** role.
      * One with the **Attendee** role.
3.  For each registration, you must **check your email(as there is no verification for a valid email)** for the 6-digit OTP code to verify your account.

### 2\. Organizer(Admin) Flow

1.  Log in as the **Admin**.
2.  On the Admin Dashboard, click **"Add Venue"** to create a location (e.g., "Main Auditorium").
3.  Click **"Create Event"** to schedule an event. You will select the venue you just created and set the time, capacity, and price.
4.  The new event will appear on your dashboard.
5.  Click **"View Attendees"** on an event card to see who has registered.
6.  On the day of the event, you can use the **"Check-In"** button on the attendees page to validate tickets.
7.  After the event's end time has passed, the **"View Report"** button will become active, allowing you to see statistics for the event.

### 3\. Attendee Flow

1.  Log out and log in as the **Attendee**.
2.  You will see the event(s) created by the Admin on your dashboard.
3.  Select a quantity and click **"Buy Ticket(s)"** to register.
4.  Navigate to the **"My Tickets"** page from the link in the header.
5.  Here you can see your unique Ticket ID. This is what the Admin will use to check you in.
6.  If you can no longer attend, you can click the **"Cancel Ticket"** button to receive a 90% refund (as long as it's more than 30 minutes before the event).
7.  30 minutes before the event starts, you will receive an automated email reminder (check your spam folder).
