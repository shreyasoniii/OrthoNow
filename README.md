This repository contains my submission for the OrthoNow Developer Assignment. The assignment consists of three tasks focused on web development, analytics tracking, and system integration.

## Task 1 – GTM Event Schema

Created a complete Google Tag Manager (GTM) and Google Analytics 4 (GA4) event tracking plan for the OrthoNow landing page.

**Includes:**
- GTM event schema
- Event triggers
- GA4 event parameters
- Booking funnel tracking
- Google Ads conversion event selection

---

## Task 2 – Landing Page

Built a responsive landing page using **HTML, CSS, and Vanilla JavaScript**.

**Features:**
- Mobile-first responsive design
- Headline and subheadline
- Two-field consultation form (Name & Phone)
- Trust section
- Form validation
- Thank-you message without page reload
- GTM `dataLayer.push()` event on successful form submission
- Optimized for a 90+ Google PageSpeed Insights mobile score

---

## Task 3 – Integration Design

Designed the integration flow between the landing page, HubSpot CRM, Karix WhatsApp Business API, and Google Ads.

The document explains:
- End-to-end architecture
- CRM contact creation/update
- WhatsApp confirmation flow
- Google Ads conversion tracking
- Failure handling and monitoring strategy

---

## Technologies Used

- HTML5
- CSS3
- JavaScript (Vanilla)
- Google Tag Manager (GTM)
- Google Analytics 4 (GA4)
- Google Ads
- HubSpot CRM
- Karix WhatsApp Business API

---

## How to Run

1. Clone or download this repository.
2. Open `index.html` in any modern web browser.
3. Fill out the form to test the landing page.
4. Open the browser console and run:

```javascript
window.dataLayer
```
