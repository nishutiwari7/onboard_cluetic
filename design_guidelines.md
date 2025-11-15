# Cluetic Onboarding Agent - Design Guidelines

## Design Approach

**Selected Approach:** Design System-Based (Material Design + Linear influence)

**Rationale:** This is a utility-focused authentication and data collection flow requiring clarity, trust, and efficiency. We'll draw from Material Design's form patterns and Linear's clean typography while maintaining Cluetic's brand identity through the gradient blue-purple color palette from the logo.

**Core Principles:**
- Trust and credibility through clean, professional design
- Minimal friction in authentication flows
- Clear visual hierarchy guiding users through multi-step processes
- Friendly yet professional tone in chat interface

---

## Typography

**Font Family:**
- Primary: Inter (Google Fonts) - for UI elements, forms, body text
- Accent: Space Grotesk (Google Fonts) - for headings and brand elements

**Type Scale:**
- Page Titles: text-4xl font-bold (Space Grotesk)
- Section Headings: text-2xl font-semibold (Space Grotesk)
- Form Labels: text-sm font-medium (Inter)
- Input Text: text-base (Inter)
- Body Text: text-base leading-relaxed (Inter)
- Helper Text: text-sm (Inter)
- Chat Messages: text-base leading-relaxed (Inter)
- Chat Timestamps: text-xs opacity-70 (Inter)

---

## Layout System

**Spacing Primitives:** Use Tailwind units of 2, 4, 6, 8, 12, and 16 throughout (p-4, m-8, gap-6, etc.)

**Authentication Pages Layout:**
- Split-screen design: 50/50 on desktop (lg:), stacked on mobile
- Left panel: Branding, value proposition, testimonial/trust signals
- Right panel: Form content centered with max-w-md
- Padding: p-8 on form container, p-12 on brand panel

**Chat Interface Layout:**
- Full-height viewport layout (h-screen)
- Fixed header: h-16 with Cluetic branding
- Scrollable message area: flex-1 with max-w-3xl centered
- Fixed input area: bottom-anchored with backdrop blur

**Form Structure:**
- Vertical spacing between form fields: space-y-6
- Input groups: space-y-2 (label + input + helper text)
- Button spacing from inputs: mt-8
- Section spacing: space-y-12

---

## Component Library

### Authentication Forms

**Input Fields:**
- Full-width with rounded-lg borders
- Height: h-12 for text inputs
- Padding: px-4 py-3
- Focus states with ring offset
- Label positioned above input with mb-2
- Error messages below input with text-sm
- Icon support for email/password fields (left-aligned with pl-12)

**Checkbox (I am not a robot):**
- Custom styled with Heroicons check icon
- Size: w-5 h-5
- Label text-sm positioned to the right with ml-3
- Container with flex items-center

**Primary Buttons:**
- Full-width on mobile, auto-width on desktop
- Height: h-12
- Padding: px-8 py-3
- Rounded-lg with font-semibold text
- Gradient background (matching logo)
- Disabled state with reduced opacity

**Secondary Buttons/Links:**
- Text-based with underline on hover
- Font-medium with tracking-wide
- Used for "Already have an account?" "Forgot password?" etc.

### OTP Verification

**OTP Input Grid:**
- 6 individual boxes in grid layout (grid-cols-6 gap-3)
- Each box: w-12 h-14 with text-center text-2xl font-bold
- Rounded-lg borders with focus ring
- Auto-advance to next box on input

**Timer Display:**
- Centered below OTP boxes with text-sm
- Countdown format: "0:45 remaining"
- Resend button appears when timer expires

**QR Code Section:**
- Centered container with max-w-xs
- QR code image with p-4 border rounded-lg
- Instructions text-sm above QR code
- Optional "Scan to login" heading text-lg font-semibold

### Chat Interface

**Message Bubbles:**
- Bot messages: Left-aligned with avatar
- User messages: Right-aligned without avatar
- Max-width: max-w-lg for readability
- Padding: px-4 py-3
- Rounded-2xl with different radii for bot (rounded-tl-sm) and user (rounded-tr-sm)
- Spacing between messages: space-y-4

**Bot Avatar:**
- Fixed size: w-10 h-10 rounded-full
- Cluetic logo image
- Positioned to the left of bot messages with mr-3

**Chat Input Area:**
- Full-width container with backdrop-blur-lg
- Input field: rounded-full with h-14
- Send button: Circular with w-10 h-10, positioned absolute right
- Padding: p-4 on container

**Skip Button (for optional questions):**
- Text button positioned below input
- text-sm with underline decoration
- "Skip this question →" with arrow icon

**Progress Indicator:**
- Subtle dots or step counter showing question progress (1/5, 2/5, etc.)
- Positioned at top of chat area
- text-xs with opacity-60

### Password Reset Flow

**Email Sent Confirmation:**
- Centered card with max-w-md
- Icon (envelope/checkmark) at top
- Heading + descriptive text
- "Back to login" link at bottom
- Spacing: space-y-6 for vertical rhythm

### Trust Elements

**Security Badges:**
- Small icons with text (lock icon + "256-bit encryption")
- Positioned in footer of auth pages
- text-xs with opacity-70

**Progress Indicators:**
- For multi-step flows (signup → OTP → chat)
- Horizontal stepper with 3 dots
- Active step highlighted, completed steps filled
- Positioned at top of form with mb-8

---

## Animations

**Use Sparingly:**
- Page transitions: Simple fade-in (duration-200)
- Form field focus: Smooth ring appearance (transition-all)
- Button hover: Subtle scale (hover:scale-105 transition-transform)
- Chat messages: Gentle slide-up on new message (animate-in slide-in-from-bottom)
- OTP auto-advance: No animation, instant focus
- Loading states: Simple spinner, no complex animations

---

## Accessibility

- All form inputs must have associated labels (not just placeholders)
- Focus states clearly visible with ring utilities
- Error messages linked to inputs via aria-describedby
- Keyboard navigation fully supported (Tab order logical)
- Chat interface keyboard accessible (Enter to send, Tab to skip button)
- Sufficient contrast ratios for all text (check against gradient backgrounds)
- Screen reader announcements for OTP timer and chat messages

---

## Icons

**Library:** Heroicons (via CDN) - exclusively

**Common Icons:**
- Lock (password fields)
- Envelope (email fields)
- Phone (phone number field)
- Eye/EyeOff (password visibility toggle)
- Check (success states, checkbox)
- X (error states, close modals)
- Clock (timer)
- Arrow Right (skip buttons, send message)
- QR Code (QR code section)

---

## Images

**Brand Panel (Auth Pages):**
- Abstract geometric pattern or gradient illustration
- Positioned as background with overlay
- Represents technology/connectivity theme
- Dimensions: Full height of left panel

**No Hero Image Required** - This is a functional app flow, not a marketing page

---

## Page-Specific Guidelines

**Sign Up Page:**
- Split layout with brand panel left, form right
- Form fields stacked vertically with consistent spacing (space-y-6)
- "I am not a robot" checkbox before submit button
- "Already have an account?" link below button
- Trust badge in footer

**Login Page:**
- Mirrored layout from sign up
- Email and password only
- "Forgot password?" link positioned to right of password label
- Google/social login options optional (below divider if included)

**OTP Verification:**
- Centered single-column layout (max-w-lg mx-auto)
- Clear heading explaining "Enter the code sent to [email]"
- OTP input grid prominent
- Timer below grid
- Resend button when timer expires
- QR code section below with divider separator
- Back link to edit email

**Chat Interface:**
- Clean, distraction-free design
- Welcome message with Cluetic branding/avatar
- Questions appear one at a time
- Input locked until previous question answered
- Skip button appears only for optional questions (website, LinkedIn, social)
- Final "Thank you" message after all responses
- Subtle "Powered by Cluetic" footer

**Forgot Password:**
- Centered card layout
- Email input only
- Clear instructions
- Confirmation state showing "Email sent" with icon
- Link back to login

---

This design framework creates a professional, trustworthy onboarding experience that balances Cluetic's brand personality with the functional requirements of authentication and data collection flows.