# GTM / GA4 Event Schema & Booking Funnel Tracking

## 1. Full Event Schema

| Event Name | GTM Trigger Type | Key Event Parameters | GA4 Purpose |
|---|---|---|---|
| `clinic_page_view` | Page View / History Change (URL contains `/clinics/`) | `clinic_name`, `clinic_city`, `page_path` | Measure clinic page traffic, Pages & Screens report, build "Clinic Visitors" audience |
| `call_now_click` | Click Trigger (Just Links, Click Class/ID = `call-now-btn`) | `clinic_name`, `page_location`, `link_text` | Track phone call intent and create engagement audiences |
| `whatsapp_click` | Click Trigger (Link URL contains `wa.me`) | `page_location`, `entry_point`, `device_category` | Measure WhatsApp engagement and build remarketing audiences |
| `generate_lead` | Form Submission Trigger (or Custom Event if JS-based form) | `form_id`, `clinic_name`, `lead_type`, `submission_status` | Track successful guide requests and lead generation performance |
| `file_download` | Click Trigger on downloadable PDF | `file_name`, `file_extension`, `clinic_name` | Measure guide downloads and content engagement |
| `blog_scroll_depth` | Scroll Depth Trigger (25%, 50%, 75%, 90%) | `scroll_percentage`, `article_title`, `page_path` | Measure blog engagement and identify highly engaged readers |
| `booking_step_complete` | Custom Event Trigger (`booking_step_complete`) | `step_number`, `step_name`, `clinic_location`, `specialty` | Build booking funnel and identify drop-off points |
| `booking_confirmed` | Custom Event Trigger (`booking_confirmed`) | `clinic_location`, `specialty`, `preferred_date` | Primary business conversion — GA4 Conversion, Google Ads Conversion |

### JSON Snippet — Sample dataLayer Payloads per Event

```json
{
  "event": "clinic_page_view",
  "clinic_name": "Downtown Dental Clinic",
  "clinic_city": "Delhi",
  "page_path": "/clinics/downtown-dental/"
}
```

```json
{
  "event": "call_now_click",
  "clinic_name": "Downtown Dental Clinic",
  "page_location": "https://example.com/clinics/downtown-dental/",
  "link_text": "Call Now"
}
```

```json
{
  "event": "whatsapp_click",
  "page_location": "https://example.com/clinics/downtown-dental/",
  "entry_point": "clinic_page_footer",
  "device_category": "mobile"
}
```

```json
{
  "event": "generate_lead",
  "form_id": "guide-request-form",
  "clinic_name": "Downtown Dental Clinic",
  "lead_type": "guide_request",
  "submission_status": "success"
}
```

```json
{
  "event": "file_download",
  "file_name": "clinic-treatment-guide.pdf",
  "file_extension": "pdf",
  "clinic_name": "Downtown Dental Clinic"
}
```

```json
{
  "event": "blog_scroll_depth",
  "scroll_percentage": 75,
  "article_title": "Top 5 Tips Before Your First Consultation",
  "page_path": "/blog/first-consultation-tips/"
}
```

---

## 2. Booking Form Funnel Tracking

**Why use `dataLayer`?**
The booking form is a JavaScript multi-step form. GTM cannot automatically detect when a user completes each step because no native browser event exists for these transitions. The front-end developer must push events into the `dataLayer` whenever a user completes a booking step. GTM then listens for these events using Custom Event Triggers and sends them to GA4.

| Step | Description |
|---|---|
| Step 1 | Clinic Location & Specialty Selected |
| Step 2 | Contact Details Submitted |
| Step 3 | Booking Successfully Confirmed |

**GTM Configuration**
- Trigger type: Custom Event
- Event name: `booking_step_complete` (Steps 1–2) and `booking_confirmed` (Step 3)
- These triggers fire a corresponding GA4 Event tag, sending `step_number` and `step_name` as event parameters (registered as custom dimensions in GA4 Admin)

### JSON Snippet — Step 1: Clinic Location & Specialty Selected

```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "clinic_and_specialty_selected",
  "clinic_location": "Downtown Delhi",
  "specialty": "Dermatology"
}
```

### JSON Snippet — Step 2: Contact Details Submitted

```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "contact_details_submitted",
  "clinic_location": "Downtown Delhi",
  "specialty": "Dermatology"
}
```

### JSON Snippet — Step 3: Booking Confirmed

```json
{
  "event": "booking_confirmed",
  "clinic_location": "Downtown Delhi",
  "specialty": "Dermatology",
  "preferred_date": "2026-07-15"
}
```

---

## 3. Funnel Exploration in GA4

To analyse user drop-off during the booking process:

1. Open **GA4 → Explore → Funnel Exploration**.
2. Create an **Open Funnel** with the following sequence:

| Funnel Step | Event / Condition |
|---|---|
| Step 1 | `booking_step_complete` where `step_number = 1` |
| Step 2 | `booking_step_complete` where `step_number = 2` |
| Step 3 | `booking_confirmed` |

GA4 will automatically display:
- Users reaching each step
- Completion rate
- Drop-off percentage
- Conversion rate between steps

The funnel can also be segmented by:
- Clinic Location
- Device Category
- Traffic Source
- City

---

## 4. Conversion Action to Import into Google Ads

**Selected Conversion Event:** `booking_confirmed`

**Reason:**
`booking_confirmed` represents a completed appointment booking, making it the most valuable business outcome and the closest event to revenue generation. Google Ads Smart Bidding (Target CPA, Maximize Conversions, etc.) performs best when optimized against meaningful business conversions rather than engagement events.

Events such as:
- `call_now_click`
- `whatsapp_click`
- `generate_lead`

indicate user interest but do not guarantee that a consultation has been booked.

**Conclusion:** `booking_confirmed` should be imported as the primary Google Ads conversion, while the other events are retained as secondary conversions for reporting and remarketing.

---

## 5. Testing & Validation

Before publishing the GTM container:

1. Use **GTM Preview Mode** to verify that each trigger fires correctly.
2. Confirm all events and parameters appear in **GA4 DebugView**.
3. Validate events in the **GA4 Realtime Report**.
4. **Publish** the GTM container only after successful testing.
