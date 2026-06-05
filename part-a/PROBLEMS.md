# IRCTC Problem Discovery — Part A

## Summary

* Total problems documented: 6 (3 given + 3 self-discovered)
* Platform explored: IRCTC (irctc.co.in)
* Devices used: Desktop Chrome and Mobile Browser

---

# Problem 1: Tatkal Booking Crashes at 10:00 AM 

## What is broken

The IRCTC server becomes extremely slow or unresponsive when Tatkal booking opens at 10:00 AM. Users experience page freezes, session timeouts, CAPTCHA resets, OTP delays, and booking failures during the most critical stage of ticket booking.

## Affected Users

* Tatkal ticket users across India
* Approximately 20–40 lakh users attempting bookings between 9:58 AM and 10:05 AM
* Users from Tier 2 and Tier 3 cities are particularly affected because train travel is often their primary transportation option

## Frequency

* Daily
* Occurs every morning when Tatkal booking opens at 10:00 AM
* High severity and recurring issue

## Current Flow — Step by Step

1. User opens IRCTC around 9:50 AM.
2. User logs in and searches for the required train.
3. User selects Tatkal quota and verifies seat availability.
4. User fills passenger details before 10:00 AM.
5. User clicks "Book Now" when Tatkal booking starts.
6. System freezes and shows a loading spinner.
7. User waits 15–45 seconds without feedback.
8. User receives a 502 error, timeout, or CAPTCHA reset.
9. User refreshes the page and is often logged out.
10. User logs in again and discovers Tatkal quota is already exhausted.

## Where Exactly It Breaks

* Steps 6–8
* The system cannot handle the sudden spike in concurrent requests.
* No queue status or progress information is shown to users.
* Repeated user refreshes and clicks increase server load, worsening the problem.

---

# Problem 2: Search Filters Do Not Work Reliably 

## What is broken

IRCTC search filters such as quota, class, seat availability, and departure time frequently fail to apply correctly. Filters often reset after navigation or display results that do not match the selected criteria.

## Affected Users

* All train search users
* Senior citizens and first-time users are particularly affected because they rely heavily on filters to simplify train selection.

## Frequency

* Occurs intermittently
* Estimated success rate: 60–70%
* More common during periods of high traffic

## Current Flow — Step by Step

1. User searches for trains between two stations.
2. Search results display 20–40 trains.
3. User selects filters such as Sleeper Class and Available Seats.
4. Results refresh after filter selection.
5. Waitlisted trains still appear despite selecting Available Seats.
6. User opens a train listing.
7. Seat status shows WL despite filter settings.
8. User returns to the search page.
9. Previously selected filters have reset.
10. User must apply filters again.

## Where Exactly It Breaks

* Steps 4–9
* Filter state is not consistently preserved.
* Availability data refreshes independently from filter settings.
* Users receive inconsistent and misleading results.

---

# Problem 3: Seat Selection Resets Randomly 

## What is broken

When users select a preferred seat or berth during booking, the selection is sometimes lost during navigation to the next step. The system either assigns another berth or switches back to automatic allocation.

## Affected Users

* Families travelling together
* Senior citizens requiring lower berths
* Users with disabilities
* Anyone selecting berth preferences manually

## Frequency

* Occurs in approximately 15–25% of booking sessions
* Higher occurrence on mobile devices

## Current Flow — Step by Step

1. User selects train and class.
2. User proceeds to seat selection.
3. Seat map loads and displays available berths.
4. User selects a preferred berth.
5. Selected berth is highlighted.
6. User clicks Proceed.
7. Passenger details page loads.
8. Selected berth is replaced by Auto Allocation or another berth.
9. User returns to the seat map.
10. Previously selected berth may already be unavailable.

## Where Exactly It Breaks

* Steps 6–8
* Seat selection state is not reliably transferred between booking screens.
* Mobile page re-renders can clear local state.
* Users lose confidence in the booking process.
