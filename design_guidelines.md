# Design Guidelines: Financial Research Sentiment Analysis Platform

## Design Approach

**Selected System**: Material Design 3  
**Justification**: This data-intensive financial analytics platform requires exceptional information hierarchy, robust component patterns for tables and forms, and clear visual feedback systems. Material Design 3 excels at content-rich applications with its emphasis on functionality, systematic spacing, and proven data visualization patterns.

**Key Design Principles**:
1. **Data Clarity**: Information hierarchy that prioritizes readability and scannability
2. **Professional Trust**: Clean, systematic design that conveys credibility for financial data
3. **Efficient Workflow**: Minimize clicks and cognitive load for filtering and analysis tasks
4. **Visual Feedback**: Clear sentiment indicators and interactive states

---

## Core Design Elements

### A. Typography

**Font Families**:
- Primary: Inter (via Google Fonts) - body text, UI elements, data tables
- Monospace: JetBrains Mono - numerical data, timestamps, code-like content

**Hierarchy**:
- H1 (Page Headers): text-4xl font-semibold tracking-tight (36px)
- H2 (Section Headers): text-2xl font-semibold (24px)
- H3 (Card Headers): text-xl font-medium (20px)
- Body Large: text-base font-normal (16px) - primary reading content
- Body Default: text-sm font-normal (14px) - table cells, descriptions
- Body Small: text-xs font-normal (12px) - metadata, timestamps, auxiliary info
- Labels: text-sm font-medium (14px) - form labels, filter headers
- Caption: text-xs font-medium uppercase tracking-wide - overline text, categories

**Data Typography**:
- Sentiment Scores: text-2xl font-bold font-mono - large numerical displays
- Table Numbers: text-sm font-mono - consistent numerical alignment
- Dates/Times: text-xs font-mono - ISO format timestamps

---

### B. Layout System

**Spacing Primitives**: Tailwind units of **2, 4, 6, 8, 12, 16**
- Micro spacing (within components): p-2, gap-2, space-x-2
- Standard component padding: p-4, p-6
- Section spacing: py-8, py-12
- Major section gaps: gap-16, py-16
- Container margins: mx-4 (mobile), mx-8 (desktop)

**Grid System**:
- Dashboard: 3-column grid for pendulums (grid-cols-1 md:grid-cols-3 gap-6)
- Articles: Single column with full-width table
- Filters: 2-4 column grid for filter inputs (grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4)

**Container Widths**:
- Max content width: max-w-7xl mx-auto (1280px)
- Narrow content: max-w-4xl (articles detail, chat messages)
- Full-width data tables: w-full with horizontal scroll on mobile

---

### C. Component Library

#### Navigation
**Top Navigation Bar**:
- Fixed header with backdrop-blur-lg bg-white/80 (glassmorphism effect)
- Height: h-16
- Logo left, navigation links center, user menu right
- Border bottom: border-b with subtle divider
- Active state: border-b-2 in accent color beneath active link

**Sidebar Navigation** (optional for future dashboard expansion):
- w-64 fixed sidebar
- Collapsible to w-16 icon-only on mobile
- Navigation items with py-3 px-4, rounded-lg hover states

#### Pendulum Visualization
**Structure**:
- Card container: rounded-xl border shadow-sm bg-white p-6
- Pendulum dial: SVG-based semicircle arc (180°)
- Needle: CSS transform rotate() based on sentiment angle (-40° to +40°)
- Score display: Centered below dial, text-3xl font-bold font-mono
- Label: Region name in text-sm font-medium text-gray-600

**Color Mapping** (sentiment states):
- Bearish (0-34): Red indicators
- Neutral (35-65): Gray indicators  
- Bullish (66-100): Green indicators

**Dimensions**:
- Dial diameter: 200px (desktop), 160px (mobile)
- Needle length: 80% of radius
- Card min-height: h-80

#### Data Tables
**Articles Table**:
- Rounded-lg border shadow-sm overflow-hidden
- Header row: bg-gray-50 border-b font-medium text-sm
- Cell padding: px-4 py-3
- Alternating rows: even:bg-gray-50/50
- Hover state: hover:bg-gray-100 transition
- Sortable columns: cursor-pointer with arrow icons
- Sticky header on scroll: sticky top-0 bg-white z-10

**Table Columns**:
1. Title (flex-1, font-medium)
2. Source (w-32, text-sm)
3. Date (w-28, text-xs font-mono)
4. Region/Asset (w-24 each, badges)
5. Sentiment Score (w-20, text-center font-mono font-bold with color)
6. Actions (w-12, icon buttons)

#### Filter Panel
**Layout**: 
- Collapsible panel above content: rounded-lg border bg-gray-50 p-6
- Grid of filter inputs: grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4
- Toggle button: "Filters" with count badge when active

**Filter Inputs**:
- Select dropdowns: h-10 rounded-md border px-3 text-sm
- Date range pickers: Two inputs side-by-side with gap-2
- Search input: h-10 rounded-md border px-4 with search icon prefix
- Clear all button: text-sm font-medium text-blue-600 hover:text-blue-700

#### Charts (Recharts)
**Time Series Chart**:
- Container: rounded-xl border shadow-sm bg-white p-6
- Chart height: h-80
- Dual Y-axes: Left (Sentiment 0-100), Right (Benchmark 0-100)
- Line styles: 
  - Sentiment: stroke-width-2 in blue
  - Benchmark: stroke-width-2 stroke-dasharray="5,5" in gray
- Grid: Subtle horizontal lines stroke-gray-200
- Legend: Bottom center, text-sm with colored squares
- Tooltip: rounded-lg shadow-lg bg-white border p-3

#### Chat Interface
**Layout**:
- Two-column split on desktop (lg:grid-cols-3):
  - Left: Filter sidebar (col-span-1)
  - Right: Chat messages + input (col-span-2)
- Mobile: Stacked single column

**Messages Container**:
- Scrollable area: max-h-[600px] overflow-y-auto
- User messages: ml-auto max-w-[80%] bg-blue-600 text-white rounded-2xl rounded-tr-sm p-4
- Assistant messages: mr-auto max-w-[80%] bg-gray-100 rounded-2xl rounded-tl-sm p-4
- Citations list: mt-4 space-y-2 with border-l-2 pl-4 for each citation
- Gap between messages: space-y-4

**Input Area**:
- Fixed bottom: sticky bottom-0 bg-white border-t p-4
- Textarea: min-h-[80px] rounded-lg border px-4 py-3 resize-none
- Send button: Absolute right-2 bottom-2 rounded-md p-2

#### Badges & Tags
**Sentiment Badges**:
- Rounded-full px-3 py-1 text-xs font-medium
- Bearish: bg-red-100 text-red-700
- Neutral: bg-gray-100 text-gray-700
- Bullish: bg-green-100 text-green-700

**Category Tags** (Region, Asset Class):
- Rounded-md px-2 py-1 text-xs font-medium border
- Multiple variations: bg-blue-50 border-blue-200 text-blue-700

#### Cards
**Standard Card**:
- rounded-xl border shadow-sm bg-white
- Padding: p-6
- Header section: pb-4 border-b mb-4
- Card title: text-lg font-semibold

**Stat Cards** (for dashboard metrics):
- Compact height: h-32
- Large number: text-3xl font-bold
- Label below: text-sm text-gray-600
- Icon in corner: absolute top-4 right-4 opacity-20

#### Buttons
**Primary Action**:
- bg-blue-600 hover:bg-blue-700 text-white
- Height: h-10
- Padding: px-6
- Rounded: rounded-lg
- Font: text-sm font-medium

**Secondary Action**:
- border border-gray-300 hover:bg-gray-50
- Same dimensions as primary

**Icon Buttons**:
- Square: w-10 h-10
- Rounded: rounded-lg
- Hover: hover:bg-gray-100

---

### D. Animations

**Minimal, Purposeful Motion**:
- Transitions: transition-colors duration-200 for hover states
- Pendulum needle: transition-transform duration-700 ease-out when score updates
- Table row hover: transition-colors duration-150
- Modal/drawer entry: Fade in opacity + slide from edge (300ms)
- Chart data updates: Recharts default animations (smooth line transitions)

**No Animations**:
- No scroll-triggered effects
- No parallax
- No decorative animations
- No loading spinners beyond simple circular indicators

---

## Page-Specific Layouts

### Dashboard Page
**Structure**:
1. Page header with title and last update time
2. Three-column pendulum grid (Global, Europe, USA)
3. Full-width sentiment vs. benchmark chart
4. Recent articles table (top 10 rows, "View All" link)

### Articles Page  
**Structure**:
1. Page header with search input (prominent)
2. Collapsible filter panel
3. Results count and sorting controls
4. Full-width data table with pagination footer

### Chat Page
**Structure**:
1. Split layout: Filters sidebar (1/3) + Chat area (2/3)
2. API key input field above chat (collapsible, saved to localStorage)
3. Sticky input at bottom
4. Citation links open in new tabs

---

## Images

**No Hero Images**: This is a functional data platform - skip marketing-style hero sections.

**Icon System**: Use Heroicons (outline for navigation, solid for states/actions)

**Data Visualization**: All charts and graphs rendered via Recharts - no static images needed.