# Domains  

- **Going Out for Food** – I enjoy exploring restaurants and eateries with friends — it’s one of my favorite ways to catch up while trying something new. The challenge is deciding where to go and keeping track of the places I’ve enjoyed.  
- **Keeping Track of Recipes** – I’ve lived in a cook-for-yourself dorm for three years and now live off-campus, so cooking is a big part of my life. Most of my inspiration comes from social media or friends, but managing and organizing recipes has always been messy.  
- **Sorority Recruitment** – As Vice President of Recruitment for MIT Panhellenic, I oversee the recruitment process for all campus sororities. Having just completed recruitment, I’ve been reflecting on the strengths and pain points of the software we used.  
- **Student Club/Organization Management** – Clubs and student orgs constantly juggle events, calendars, meetings, and communication. A dedicated system could make coordination smoother for both leaders and members.  
- **Travel Planning** – Trips — from quick afternoon outings to full spring break vacations — are often coordinated over text threads and spreadsheets. A collaborative way to manage itineraries, budgets, and logistics would make group travel easier.  
- **Student Finances** – Students face unique financial challenges, from splitting bills with roommates to managing tuition, internships, and savings. Existing tools don’t fully address the realities of student life.  
- **MIT Reimbursement Process** – Getting reimbursed for student org or research expenses through Atlas is time-consuming. A more streamlined system could save time and reduce frustration for both students and administrators.  
- **MIT Digital Marketplace** – Over the years, several attempts have been made to create an MIT-specific marketplace for clothes, furniture, and other items, but none have gained lasting traction. This remains a valuable unmet need.  
- **Media Tracking** – I read books, articles, and newsletters, and watch movies, but I don’t have a cohesive way to track what I’ve finished, what I want to start, or what I’d recommend to others.  
- **Clothing Borrowing Network** – Friends often borrow outfits, but coordinating through texts is clunky. A structured way to lend, borrow, and keep track of items could make the process more reliable and fun.  

---

# Problems  

## Restaurant Tracker  

- **Deciding Which Restaurant to Visit with Friends** – Coordinating with friends on when/where to eat is chaotic, often defaulting to the same spots.  
  - *Selected:* Going out is social, but the hardest part is decision-making — everyone has preferences, dietary restrictions, or budget constraints, and indecision wastes time. This problem is common among students and has clear, solvable constraints (preferences, history, voting).  

- **Tracking Restaurants I’ve Visited** – Hard to remember where I’ve already been, what I liked, or what I’d recommend to others.  
  - *Excluded:* Already solved decently by tools like Google Maps’ “starred places.” Limited novelty.  

- **Discovering New Restaurants** – There are tons of places around Cambridge/Boston, but Yelp/Google reviews don’t capture personal taste or social trust.  
  - *Excluded:* Feels too close to Yelp/Google scale; hard to differentiate meaningfully.  

---

## Recipes / Cooking  

- **Keeping Track of Recipes** – Recipes are scattered across screenshots, Instagram/TikTok saves, and text messages, making them hard to find when I want them.  
  - *Excluded:* Already addressed well by platforms like Notion; less unique.  

- **Sharing Recipes with Friends** – When friends ask for a recipe, I usually just send a messy screenshot (or nothing at all); no easy way to keep a shared collection.  
  - *Excluded:* Could be rolled into a “notes + sharing” feature; not distinct enough.  

- **Adding Notes on Recipes** – When I tweak a recipe (less salt, more spice), I don’t have a good system to save those changes for next time.  
  - *Selected:* Many people adapt recipes, but existing platforms (NYT Cooking, AllRecipes, Pinterest) rarely make personal annotations easy or shareable. This problem is well-scoped and dynamic (notes evolve over time), and a solution would bring daily utility to cooking.  

---

## Sorority Recruitment  

- **Event Scheduling & Planning** – Juggling PNMs, chapter schedules, and recruitment events creates logistical headaches, especially last-minute conflicts.  
  - *Excluded:* Useful, but overlaps with many generic calendar/event tools (Google Calendar, Airtable).  

- **Communications with PNMs and Chapters** – Information gets lost across GroupMe, email, and printed handouts; PNMs are often confused.  
  - *Selected:* Recruitment depends on smooth, timely communication. Miscommunication frustrates PNMs, stresses chapters, and complicates logistics. This is a high-impact, real-world problem where even small improvements (centralized messaging, reminders, FAQs) would matter.  

- **Printed Materials** – Recruitment still relies on printing physical lists and nametags. Multiple hours are lost every evening of recruitment to manually print, cut, and stuff.  
  - *Excluded:* Important but more of a “digitization” issue than a dynamic software challenge.  

---

# Stakeholders  

## Deciding Which Restaurant to Visit with Friends  

- **Primary Diner (Users)** – Person initiating/leading decision of where to eat.  
- **Friends** – Others who join for the meal and contribute preferences.  
- **Restaurant Owners/Staff** – Indirect stakeholders affected by how often the groups choose their restaurant.  

*Impact:* The primary diner often feels the burden of decision-making, especially when balancing group preferences, dietary restrictions, and budgets. Friends can experience frustration or decision fatigue when discussions drag on, reducing enthusiasm for the outing. Restaurants indirectly benefit when discovery and decision processes steer groups their way; smaller/local restaurants in particular may lose visibility without better decision tools.  

---

## Adding Notes on Recipes  

- **Home Cooks (Users)** – Students and others who cook regularly and want to adjust recipes.  
- **Friends/Family** – People who share or receive adapted recipes.  
- **Recipe Creators/Publishers** – Original authors or platforms hosting recipes.  

*Impact:* Home cooks benefit most directly — they can refine and remember personal tweaks, reducing frustration and waste. Friends and family gain from easier sharing of reliable, tested recipes. Recipe creators may have mixed impacts: some might see annotations as derivative, while others could benefit from feedback loops and stronger community engagement around their recipes.  

---

## Communications with PNMs and Chapters  

- **PNMs (Potential New Members)** – Students going through recruitment who rely on clear, timely info.  
- **Chapter Recruitment Chairs** – Sorority leaders managing schedules, events, and communications.  
- **Panhellenic Recruitment Execs** – Officers overseeing and ensuring the process runs smoothly.  

*Impact:* PNMs are most affected — unclear communication leads to stress, missed events, or a negative impression of recruitment. Chapter recruitment chairs face logistical chaos when information doesn’t flow properly, creating conflicts and miscoordination. Panhel execs feel the pressure from both sides: ensuring fairness, minimizing confusion, and keeping the process equitable across all chapters.  

---

# Evidence & Comparables  

## Deciding Which Restaurant to Visit with Friends  

**Evidence**  
- [Choice overload – Caltech](https://www.caltech.edu/about/news/scientists-uncover-why-you-cant-decide-what-order-lunch-83881) – Explains cognitive overload in meal/venue choice, highlighting why groups often struggle to decide.  
- [What Drives Restaurant Choice – Study](https://pmc.ncbi.nlm.nih.gov/articles/PMC7503372/) – Empirical evidence that price, cuisine, and service drive decisions, showing clear multi-criteria needs.  
- [Consensus Costs in Group Decisions](https://www.sciencedirect.com/science/article/abs/pii/S0020025522016097) – Demonstrates why groups get “stuck,” validating demand for decision-support.  
- [Gen Z Turns to TikTok for Food Discovery](https://www.forbes.com/sites/johnkoetsier/2024/03/11/genz-dumping-google-for-tiktok-instagram-as-social-search-wins/) – Shows students increasingly rely on social discovery over Google/Yelp.  
- [Time Wasted Deciding Meals](https://www.delicious.com.au/food-files/article/study-reveals-we-spend-too-much-time-deciding-eat/76g0qv2a) – Survey reports Americans spend ~132 hours/year debating food choices.  

**Comparables**  
- [Google Maps Lists](https://www.google.com/maps) – Lets friends save/share lists; weak on multi-user preference aggregation.  
- [Yelp Collections](https://www.yelp.com/collections) – Public curation, but limited tools for private group decision-making.  
- [Beli](https://beliapp.com/) – Social bookmarking for places; weak on budget/diet constraint handling.  
- [GroupMe](https://groupme.com) – Common fallback, but noisy and unstructured for decision-making.  

---

## Adding Notes on Recipes  

**Evidence**  
- [College Students Cook Often](https://www.sciencedirect.com/science/article/abs/pii/S1878450X21000020) – Study shows many students frequently cook, creating real annotation needs.  
- [Cooking Education Boosts Skills](https://pmc.ncbi.nlm.nih.gov/articles/PMC10563711/) – Education improves frequency/skills, supporting the importance of repeatability.  
- [Home Cooking and Diet Quality](https://pmc.ncbi.nlm.nih.gov/articles/PMC8728746/) – Home cooking links to better diets, so recipe recall contributes to health.  
- [Food Waste Statistics – USDA](https://www.usda.gov/foodwaste/faqs) – Better recipe recall/notes could reduce waste from failed or forgotten tweaks.  

**Comparables**  
- [NYT Cooking Notes](https://cooking.nytimes.com/) – Strong annotation culture, but locked to their ecosystem.  
- [Paprika Recipe Manager](https://www.paprikaapp.com/) – Power-user features, but clunky sharing outside the app.  
- [AnyList](https://www.anylist.com/) – Good for shared lists/meal plans, but less on versioned notes.  
- [Samsung Food](https://www.samsungfood.com/) – Cross-platform saving; weaker per-attempt notes.  

---

## Communications with PNMs and Chapters  

**Evidence**  
- [NPC Recruitment Counselor Manual](https://npcwomen.org/login/college-panhellenics/recruitment-main/recruitment-counselor-training-guide/) – Shows complexity of compliance for counselor communication.  
- [Panhellenic Recruitment Rules – MIT](https://docs.google.com/document/d/1lZ7fFn6P5Ayr4P_avoN-uGFjda9vZbx26_Z_YG69uMQ/edit?usp=sharing) – Example of strict rules on PNMs/chapter interactions.  
- [Reminder Efficacy Study](https://www.sciencedirect.com/science/article/abs/pii/S016726812100408X) – Timely nudges improve attendance/compliance in other contexts.  

**Comparables**  
- [CampusDirector](https://www.phiredup.com/campusdirector/) – Centralized registration/logistics; weak on real-time, PNM-friendly messaging.  
- [ChapterBuilder](https://phiredup.com/chapterbuilder/) – Recruitment CRM, but not built for Panhel-wide formal recruitment rules.  
- [OmegaOne](https://www.omegafi.com/solutions/omegaone) – Strong for internal ops, not recruitment-specific communication.  
- [GreekTrack](https://greektrack.com/) – Broad management suite; lacks recruitment-focused, compliance-aware comms.  

---

# Features  

## Deciding Which Restaurant to Visit with Friends  

- **Smart Group Polls** – Each group member can vote on a shortlist of restaurants, with the system surfacing the best option based on preferences like cuisine, distance, and budget. This reduces the “decision fatigue” for the primary diner and ensures all friends feel heard.  
- **Dietary & Budget Filters** – Users set dietary restrictions (vegan, halal, gluten-free) and budget limits ahead of time; restaurants that don’t meet these constraints are automatically filtered out. This removes friction for the friends/group members, making the final decision faster and more inclusive.  
- **Restaurant History Tracker** – The app remembers where the group has already been and how each member rated it, so the group doesn’t keep defaulting to the same place unless they really liked it. This benefits both diners (less repetition) and restaurant owners (increasing discovery of new spots).  

---

## Adding Notes on Recipes  

- **Inline Recipe Annotations** – Users can highlight specific steps or ingredients and attach notes (“use less salt here,” “double garlic”). Home cooks get a personalized version of the recipe without having to rewrite the whole thing.  
- **Version Control for Recipes** – Every time someone makes changes, a new “version” is saved with notes about what was different and how it turned out. This helps friends/family easily adopt tried-and-true variations and preserves the original creator’s recipe for attribution.  
- **Shared Recipe Notebooks** – Friends can create collaborative collections where everyone contributes recipes and notes. This strengthens community cooking and makes sharing more meaningful for friends/family, while giving home cooks a structured place to store their tweaks.  

---

## Communications with PNMs and Chapters  

- **Centralized Messaging Hub** – A single platform where all announcements, FAQs, and reminders live — no more toggling between GroupMe, email, and texts. This ensures PNMs don’t miss critical info and reduces duplicate effort for chapter chairs.  
- **Automated Event Reminders** – PNMs receive timed reminders before each event via their preferred channel (SMS, push notification, or email). This increases attendance, lowers stress for PNMs, and saves Panhel execs from sending last-minute mass messages.  
- **Compliance Guardrails** – Built-in features that prevent chapters from sending non-compliant or early communications (e.g., blocking unsanctioned direct contact). This protects Panhel execs from rule violations, reassures PNMs of fairness, and supports chapter recruitment chairs by clarifying what’s allowed.  
