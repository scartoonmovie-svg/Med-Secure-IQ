# Med-Secure IQ — Project Document
**Author:** Shaurya Verma
**Competition:** Ayushman Bharat Diwas Tech Competition 2026
**Category:** Web Development (HealthTech + Cyber Security)
**Theme:** Code. Secure. Care.

---

## 1. THE PROBLEM I AM SOLVING

### What is Polypharmacy?
Polypharmacy means taking 5 or more medicines at the same time. In India, this is extremely common — especially among elderly patients with diabetes, hypertension, and heart disease.

### The Real Problem Nobody Talks About
When a patient visits 3-4 different doctors, each doctor prescribes medicines without knowing what the other doctor prescribed. The patient ends up taking 8-10 medicines simultaneously. Some of these medicines are chemically dangerous when combined. Some cancel each other out. Some cause serious side effects or even death.

**This is called a Drug-Drug Interaction (DDI) — and it kills people silently.**

### Scale of the Problem in India
- Over 50% of elderly Indians take 5+ medicines simultaneously
- India has 1 doctor per 1,456 patients (WHO standard is 1 per 1,000)
- Doctors spend an average of 2 minutes per patient — no time to check interactions
- No free, offline, privacy-safe tool exists for Indian patients to check this themselves
- Polypharmacy-related adverse drug reactions cause thousands of preventable deaths annually

### Real Example
A patient with diabetes and heart disease visits:
- Doctor 1 (cardiologist): prescribes Warfarin (blood thinner)
- Doctor 2 (general physician): prescribes Aspirin (pain/fever)

**Result:** Aspirin + Warfarin together = massively increased bleeding risk. Can cause internal bleeding or death. Neither doctor knew about the other's prescription.

---

## 2. MY SOLUTION — Med-Secure IQ

**Med-Secure IQ** is a web-based Polypharmacy Defense Portal that detects dangerous drug combinations in real-time, entirely in the browser, with zero data sent to any server.

### Core Innovation
The entire drug interaction engine runs client-side in JavaScript. I built a hash-map lookup system that checks every pair of medicines in O(1) time. When you add a medicine, it instantly checks all combinations against a database of 70+ real drug interactions sourced from WHO, CDSCO India, and Medscape.

### What Makes It Different From Existing Apps
| Feature | Other Apps | Med-Secure IQ |
|---|---|---|
| Works offline | No | Yes |
| Sends data to server | Yes | Never |
| Free to use | Paid/limited | 100% free |
| India-specific drugs | No | Yes (CDSCO sourced) |
| Real-time detection | No | Yes |
| Chemical Load Score | No | Yes (custom algorithm) |

---

## 3. HOW IT WORKS — Technical Explanation

### Architecture
Single-page web application. No backend. No database. No login. No server.

**Files:**
- `index.html` — complete page structure (4 sections)
- `css/styles.css` — custom dark theme, all hand-written CSS
- `js/interactions-db.js` — drug database + interaction lookup engine
- `js/engine.js` — all application logic

### The Drug Interaction Engine
I built a JavaScript hash map (dictionary) where every drug pair is stored as a key:

```
key = "aspirin|warfarin"
value = { severity: "danger", effect: "...", source: "WHO" }
```

When user adds Medicine A and Medicine B, the engine does:
```javascript
checkInteraction("Aspirin", "Warfarin")
// Returns: { severity: "danger", effect: "Greatly increases bleeding risk..." }
```

This runs in O(1) constant time — instant, no delay.

### Chemical Load Score Algorithm
I designed a custom scoring algorithm:

```
Score = 0
+ 10 points if 2+ medicines
+ 15 points if 4+ medicines
+ 20 points if 6+ medicines
+ 15 points if 8+ medicines
+ 20 points per DANGEROUS interaction found
+ 8 points per WARNING interaction found
Maximum = 100
```

Score 0-30 = Green (Safe)
Score 31-60 = Yellow (Caution)
Score 61-100 = Red (Danger)

### Risk Classification
Every drug pair is classified as:
- DANGEROUS (Red) — contraindicated, life-threatening, avoid completely
- WARNING (Yellow) — significant interaction, monitor closely
- SAFE (Green) — no known significant interaction

---

## 4. FEATURES BUILT

### Section 1: Patient Profile
- Enter name, age, gender, blood group
- Known allergies (comma separated)
- Existing conditions (diabetes, hypertension, etc.)
- Profile chips display after saving
- Profile data included in QR export

### Section 2: Crisis Dashboard
- Medicine input with autocomplete (100+ drug names)
- Quick-add buttons for 8 common medicines
- Animated Chemical Load Score gauge (Chart.js doughnut)
- Active medicines list with remove button
- Clear all button
- Export QR Medicine Passport button

### Section 3: Interaction Engine
- Checks every pair of added medicines
- Summary cards: total pairs, dangerous, warnings, safe
- Individual interaction cards sorted by severity (danger first)
- Each card shows: drug names, effect description, source citation
- Reference table: 20 pre-loaded high-risk combinations

### Section 4: Secure Vault
- Zero Server Architecture explanation
- Client-Side Processing explanation
- Phishing Awareness section
- Data Privacy by Design (DPDP Act 2023 alignment)
- Live Security Audit button — runs 7 real checks on the session

### QR Medicine Passport
- Generates scannable QR code with medicine list
- Includes patient name, blood group, allergies if profile filled
- No personal ID, no Aadhaar, no sensitive data
- Patient can show this to any doctor

---

## 5. TECHNOLOGY USED

### Languages
- HTML5 — page structure and semantic markup
- CSS3 — complete custom dark theme, animations, responsive layout
- JavaScript (Vanilla ES5/ES6) — all logic, no frameworks

### Libraries (CDN, no installation needed)
- Chart.js v4.2.1 — animated doughnut gauge for Chemical Load Score
- QRCode.js — generates QR code in browser

### No frameworks used
No React, no Vue, no Angular, no Bootstrap, no jQuery. Everything written from scratch. This demonstrates deeper technical understanding.

### Deployment
Netlify (static hosting, automatic HTTPS, free tier)

---

## 6. DATA SOURCES (Research)

All drug interactions are sourced from:

1. **WHO Essential Medicines List** — World Health Organization's official drug interaction guidelines
2. **CDSCO India** — Central Drugs Standard Control Organisation, India's drug regulatory authority
3. **Medscape Drug Interactions** — Clinical pharmacology reference used by doctors worldwide
4. **Clinical Pharmacology** — Standard medical reference for drug-drug interactions

### Key Interactions Documented
| Drug A | Drug B | Risk | Effect |
|---|---|---|---|
| Aspirin | Warfarin | DANGER | Fatal bleeding risk |
| Sildenafil | Nitroglycerine | DANGER | Severe hypotension, can be fatal |
| Tramadol | Fluoxetine | DANGER | Serotonin syndrome, potentially fatal |
| Morphine | Diazepam | DANGER | Respiratory arrest risk |
| Digoxin | Amiodarone | DANGER | Digoxin toxicity |
| Metoprolol | Verapamil | WARNING | Bradycardia, heart block |
| Levothyroxine | Iron | WARNING | Reduced thyroid absorption |
| Omeprazole | Iron | WARNING | Reduced iron absorption |

---

## 7. CYBER SECURITY FEATURES

This is why the project fits the "Cyber Security" category of the competition:

### Zero Server Architecture
Every drug interaction check runs in the browser. No data is transmitted to any server. Open DevTools, go to Network tab, add a medicine — zero network requests are made.

### Privacy by Design
Aligned with India's Digital Personal Data Protection Act 2023 (DPDP Act). Principles followed:
- Data Minimization: only process what user types
- Purpose Limitation: data used only for interaction checking
- Storage Limitation: nothing stored to localStorage or database

### No Cookies, No Tracking
No analytics, no fingerprinting, no session tracking.

### Phishing Awareness
The Secure Vault section educates users about fake medical apps that steal health data. Red flags to watch for: apps asking for Aadhaar, login, or "cloud sync" for basic medicine checks.

### HTTPS
Deployed on Netlify with automatic TLS 1.3 encryption.

---

## 8. COMPETITION ALIGNMENT

### Theme: "Code. Secure. Care."
- CODE: Custom JavaScript interaction engine, hash-map algorithm, Chemical Load Score formula
- SECURE: Zero server architecture, Privacy by Design, DPDP Act alignment, phishing awareness
- CARE: Solves real polypharmacy problem, protects patients from dangerous drug combinations

### Judging Criteria Match
| Criteria | How I Address It |
|---|---|
| Originality | No other free offline polypharmacy checker exists in India |
| Technical Depth | Custom JS engine, hash-map lookup, scoring algorithm |
| Real-World Impact | Directly prevents drug interaction deaths |
| Cyber Security | Zero data transmission, Privacy by Design, DPDP Act |
| Presentation | Live demo: add Aspirin + Warfarin, watch danger alert appear instantly |

---

## 9. LIVE DEMO SCRIPT (For Competition Day)

1. Open the website
2. Fill Patient Profile: Name, Age 65, Blood Group B+, Condition: Diabetes
3. Go to Crisis Dashboard
4. Click quick-add: Aspirin
5. Click quick-add: Warfarin
6. Watch: gauge jumps to red, "DANGEROUS" card appears instantly
7. Add Metformin — gauge updates, new pairs checked
8. Add Omeprazole — WARNING card appears (reduces iron absorption)
9. Click "Export QR Card" — show QR code with medicine list
10. Scroll to Secure Vault, click "Run Security Audit"
11. Show judges: 7 green checks, zero data transmitted

**Key line to say:** "Every check you just saw happened entirely in your browser. I can disconnect the internet right now and it still works perfectly. Your medical data never left your device."

---

## 10. FILE STRUCTURE

```
mediguard-ai/
  index.html              — complete single-page application
  css/
    styles.css            — custom dark theme (black + electric blue)
  js/
    interactions-db.js    — drug database + hash-map lookup engine
    engine.js             — all application logic
  assets/
    favicon.svg           — site icon
  manifest.json           — PWA manifest
  netlify.toml            — deployment config
  PROJECT_DOCUMENT.md     — this file
```

---

*Built by Shaurya Verma for Ayushman Bharat Diwas Tech Competition 2026*
*Code. Secure. Care.*
