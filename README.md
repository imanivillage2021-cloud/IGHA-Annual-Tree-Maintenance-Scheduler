import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { CalendarDays, Clock, User, Mail, StickyNote, ShieldCheck } from "lucide-react";
import { motion } from "framer-motion";

export default function TreeMaintenanceScheduler() {
  const [selectedDates, setSelectedDates] = useState(["", "", ""]);
  const [selectedDay, setSelectedDay] = useState(null);
  const [selectedTime, setSelectedTime] = useState(null);
  const [slotNotes, setSlotNotes] = useState("");
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [submitted, setSubmitted] = useState(false);
  const [bookedSlots, setBookedSlots] = useState([]);
  const [adminView, setAdminView] = useState(false);
  const [bookings, setBookings] = useState([]);

  const weekdays = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"];

  const timeSlots = [
    { label: "9:45 AM - 10:30 AM", value: "slot1", start: "094500", end: "103000" },
    { label: "10:45 AM - 11:30 AM", value: "slot2", start: "104500", end: "113000" },
    { label: "11:45 AM - 12:30 PM", value: "slot3", start: "114500", end: "123000" },
    { label: "12:45 PM - 1:30 PM", value: "slot4", start: "124500", end: "133000" },
    { label: "1:45 PM - 2:30 PM", value: "slot5", start: "134500", end: "143000" },
    { label: "2:45 PM - 3:30 PM", value: "slot6", start: "144500", end: "153000" }
  ];

  const handleDateChange = (index, value) => {
    const updated = [...selectedDates];
    updated[index] = value;
    setSelectedDates(updated);
  };

  // ICS GENERATION (VALID & CLEAN)
  const generateICS = () => {
    const slot = timeSlots.find((s) => s.value === selectedTime);
    if (!slot || !selectedDates[0]) return;

    const date = selectedDates[0].replace(/-/g, "");

    const vtimezone = `BEGIN:VTIMEZONE
TZID:America/Chicago
BEGIN:STANDARD
DTSTART:19701101T020000
RRULE:FREQ=YEARLY;BYMONTH=11;BYDAY=1SU
TZOFFSETFROM:-0500
TZOFFSETTO:-0600
TZNAME:CST
END:STANDARD
BEGIN:DAYLIGHT
DTSTART:19700308T020000
RRULE:FREQ=YEARLY;BYMONTH=3;BYDAY=2SU
TZOFFSETFROM:-0600
TZOFFSETTO:-0500
TZNAME:CDT
END:DAYLIGHT
END:VTIMEZONE`;

    const event = `BEGIN:VEVENT
UID:${Date.now()}@imani-green
DTSTAMP:${date}T${slot.start}
SUMMARY:Imani Green Health Advocate Annual Tree Maintenance
DTSTART;TZID=America/Chicago:${date}T${slot.start}
DTEND;TZID=America/Chicago:${date}T${slot.end}
DESCRIPTION:${slotNotes.replace(/
/g, "\
")}
END:VEVENT`;

    const ics = `BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//Imani Green//Tree Scheduler//EN
${vtimezone}
${event}
END:VCALENDAR`;

    const blob = new Blob([ics], { type: "text/calendar;charset=utf-8" });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = `imani-tree-maintenance-${date}.ics`;
    link.click();
  };

  const handleSubmit = () => {
    if (selectedDay && selectedTime && name && email && selectedDates.every((d) => d)) {
      const newBooking = {
        name,
        email,
        preferredDay: selectedDay,
        time: timeSlots.find((t) => t.value === selectedTime)?.label,
        dates: selectedDates,
        notes: slotNotes
      };

      setBookings([...bookings, newBooking]);
      setBookedSlots([...bookedSlots, selectedTime]);
      setSubmitted(true);
      generateICS();
    }
  };

  const minDate = "2026-06-15";
  const maxDate = "2026-11-10";

  // ADMIN VIEW
  if (adminView) {
    return (
      <div className="p-6 max-w-4xl mx-auto space-y-6">
        <h1 className="text-3xl font-bold flex items-center gap-2"><ShieldCheck className="w-6 h-6" /> Admin Dashboard</h1>
        {bookings.length === 0 ? (
          <p>No bookings yet.</p>
        ) : (
          bookings.map((b, index) => (
            <Card key={index} className="p-4 shadow">
              <p><strong>Name:</strong> {b.name}</p>
              <p><strong>Email:</strong> {b.email}</p>
              <p><strong>Preferred Day:</strong> {b.preferredDay}</p>
              <p><strong>Time Slot:</strong> {b.time}</p>
              <p><strong>Selected Dates:</strong> {b.dates.join(", ")}</p>
              <p><strong>Notes:</strong> {b.notes || "None"}</p>
            </Card>
          ))
        )}
        <Button onClick={() => setAdminView(false)}>Back to Scheduler</Button>
      </div>
    );
  }

  // USER SCHEDULER VIEW
  return (
    <div className="p-6 max-w-3xl mx-auto space-y-6">
      <Button onClick={() => setAdminView(true)} className="bg-purple-600 text-white w-full py-2">Admin Dashboard</Button>

      <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="text-lg text-center text-gray-700 leading-relaxed">
        We’re excited to care for your tree together! Attendance is not required, but it is always highly welcomed and appreciated.
      </motion.p>

      <motion.h1 initial={{ opacity: 0, y: -10 }} animate={{ opacity: 1, y: 0 }} className="text-3xl font-bold text-center mb-4">
        Tree Maintenance Visit Scheduler
      </motion.h1>

      {!submitted ? (
        <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="space-y-6">
          {/* NAME */}
          <Card className="shadow-lg rounded-2xl p-4"><CardContent>
            <h2 className="text-xl font-semibold flex items-center gap-2 mb-3"><User /> Your Name</h2>
            <input type="text" className="w-full border rounded p-2" value={name} onChange={(e) => setName(e.target.value)} />
          </CardContent></Card>

          {/* EMAIL */}
          <Card className="shadow-lg rounded-2xl p-4"><CardContent>
            <h2 className="text-xl font-semibold flex items-center gap-2 mb-3"><Mail /> Your Email</h2>
            <input type="email" className="w-full border rounded p-2" value={email} onChange={(e) => setEmail(e.target.value)} />
          </CardContent></Card>

          {/* DATES */}
          <Card className="shadow-lg rounded-2xl p-4"><CardContent>
            <h2 className="text-xl font-semibold flex items-center gap-2 mb-3"><CalendarDays /> Select 3 Potential Dates (Jun 15 – Nov 10)</h2>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
              {selectedDates.map((date, index) => (
                <input key={index} type="date" min={minDate} max={maxDate} className="w-full border rounded p-2" value={date} onChange={(e) => handleDateChange(index, e.target.value)} />
              ))}
            </div>
          </CardContent></Card>

          {/* DAY */}
          <Card className="shadow-lg rounded-2xl p-4"><CardContent>
