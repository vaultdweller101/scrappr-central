# Scrappr: Bridging the Gap Between Ideation and Drafting in the Digital Age

**By The Scrappr Team**

## Introduction

Writing is often romanticized as a solitary act of genius—a continuous flow of inspiration from the mind to the page. However, for most writers, the reality is far more fragmented. The modern writing process is a chaotic dance between ideation, research, drafting, and editing. Writers constantly oscillate between the "architect" mindset (structured planning) and the "gardener" mindset (organic discovery).

In our initial exploration of this domain, we identified a critical friction point: the disconnection between where ideas are born and where they are finalized. Writers frequently struggle to capture fleeting thoughts without disrupting their current workflow. They might have a brilliant idea for a future chapter while editing a current one, or they might stumble upon a crucial source while browsing the web, only to lose the thread of their original argument when they switch contexts to save it.

This project, **Scrappr**, was born from the desire to solve this fragmentation. We set out to create a system that acts not just as a repository for ideas, but as an active participant in the writing process—a tool that helps writers synthesize their disparate thoughts and retrieve them exactly when they are needed most. This blog post details our journey from user research to a fully implemented prototype, highlighting the insights, pivots, and evaluations that shaped Scrappr.

## Problem Statement

Defining the core problem was an iterative process that evolved alongside our understanding of our users.

Initially, we operated under the assumption that existing tools were simply too polarized: either offering too much control (like a blank Google Doc) or taking too much control away (like generative AI writing the whole text for you). We framed our initial problem statement as: *"Writers want to be able to exercise their full writing potential through external guidance and feedback... however, existing tools either give you too fine-grained or too coarse-grained control."*.

However, feedback from our peers and critique sessions revealed that this was too broad. Through further research, we realized the issue wasn't just about "control," but about the **synthesis and integration of ideas**. Writers didn't just need help writing; they needed help connecting the dots between their scattered notes and their final draft.

We refined our problem statement to focus on this disconnection:
*"Writers tend to have problems synthesizing ideas and putting them on paper. While existing tools can help users note down ideas, integrating them into the text is still something writers need to do manually. Moreover, both of these processes currently take part over separate mediums, introducing a delay during the writing process, disrupting chains of thought and contributing to writer's block."*.

By the final stages of our project, we honed this even further to address the specific tension regarding AI:
*"Writers tend to have problems synthesizing ideas and putting them on paper. Writers however hesitate to embrace automated tools that take away their creative freedom."*.

This final statement anchored our design philosophy: we needed to build a tool that assisted with synthesis without usurping the writer's voice.

## User Research

To ground our system in reality, we conducted extensive user research involving over 20 interviews, surveys with over 100 responses, contextual inquiries, and expert consultations.

### Methodology

Our research approach was multi-faceted:

1.  **Semi-Structured Interviews:** We interviewed a diverse range of writers, from creative writers and English professors to STEM students writing research papers. We focused on their "writing environment," their process from ideation to final result, and their specific annoyances.
2.  **Contextual Inquiry:** We observed writers in their natural element—watching them write papers or organize notes in real-time. This revealed behaviors they often forgot to mention in interviews, such as the sheer number of tabs they kept open.
3.  **Think-Aloud Protocols:** We asked users to perform "flash-writes" while vocalizing their thoughts. This was crucial for identifying the immediate, moment-to-moment frustrations of the writing process.
4.  **Expert Interviews:** We spoke with leaders of writing communities, such as the Dead Writers Society, and professors of writing programs. These experts provided high-level insights into the emotional cycles of writing and the balance between structure and intuition.

### Key Findings and Personas

Our research led to the creation of primary personas to guide our design:

  * **Jordan (The Creative Writer):** Jordan wants to put her inner world on paper but struggles to capture and keep ideas. She is often annoyed by tools that are "too coarse" (like generic LLMs) or "too fine" (like simple spellcheckers).
  * **Riley (The Academic Writer):** Riley's goal is to convey discovery and knowledge. He struggles with organizing information and ensuring his ideas flow logically to the reader.

We discovered that while writers have different outputs, their struggles with **ideation** are universal. A "process map" we developed showed a common loop: writers open a document, stare at a blank page, haphazardly select an idea, write, and then often close the tab in frustration.

Crucially, we learned that writers often oscillate between "Architect" (planned) and "Gardener" (organic) approaches. A rigid tool frustrates the gardener; a chaotic tool hinders the architect. Scrappr needed to support *both* by allowing organic note-taking that could be structurally retrieved later.

We also found a strong "Anti-AI" sentiment among creative writers who felt that LLMs took away from their individuality. This confirmed our decision to use AI for *retrieval and suggestion* (Level 2/3 assistance) rather than content generation (Level 4).

## Design Goals

Based on our research, we articulated specific design goals to ensure Scrappr addressed user needs directly.

### Goal 1: Clarity and Conveyance

**The system should help writers evaluate if their idea conveyance is clear and understood.**.
Our interviews revealed that writers often "have the whole context in their head" and struggle to see if it translates to the page. They want feedback on tone and message, not just grammar. For example, one user noted they constantly question their "word choices, sentence structure, and tone".

### Goal 2: Recall and Storage

**The system should assist the writer in recalling and storing important notes and sources.**.
We observed that writers often lose ideas because noting them down requires switching contexts (e.g., leaving the document to open a notes app). We found that "having a way to save notes while in the process would save a lot of possible transition time and possible loss of ideas". Users like "Bobby" explicitly asked for something minimal to encourage organization.

### Goal 3: Customizability

**Writers should be able to choose the type of feedback that they receive.**.
Since writers have different processes, a "one size fits all" approach fails. Some want feedback on plot holes, others on character arcs or grammar. Customizability allows the tool to adapt to the writer's current "mode" (e.g., drafting vs. editing).

## System Design and Implementation

Scrappr evolved significantly from our initial concepts. We moved from a standalone web app to a more integrated ecosystem consisting of a **Browser Extension** and a companion **Mobile App**.

### Architecture

We adopted a robust tech stack to ensure cross-platform synchronization and real-time responsiveness:

  * **Frontend:**
      * **Browser Extension:** Built with **React** and **TypeScript**, using `vite` for building. It uses browser APIs like `navigator.clipboard` to access user context.
      * **Mobile App:** Built with **React Native** to facilitate on-the-go idea capture, specifically leveraging voice inputs.
  * **Backend:**
      * **Firebase:** We utilized Firebase for our database and authentication. This allowed for real-time syncing of notes between the mobile app (where ideas might be captured via voice while walking) and the browser extension (where ideas are used while writing).
      * **Storage:** We used a combination of local browser storage for speed and Firestore for persistence.

*Figure 1 below illustrates the high-level architecture of the Scrappr ecosystem.*

**Figure 1: System Architecture**

```
       [ React Native Mobile App ]               [ React Browser Extension ]
                  |                                      |
                  | (Voice & Text Notes)                 | (Contextual Suggestions)
                  v                                      v
        +-------------------------------------------------------------+
        |                       Google Firebase                       |
        |              (Firestore Database & Authentication)          |
        +-------------------------------------------------------------+
```

*[Derived from system description in source: 2513-2519]*

### Key Features and Interactions

1.  **Seamless Note Capture (The "Micronote"):**
    Inspired by research into "Micronotes", we designed Scrappr to accept quick, short inputs. Users can jot down a quick thought or record a voice note on their phone. These are immediately synced to the cloud. On the browser, users can highlight text on any webpage and save it to Scrappr via the extension, effectively gathering "scraps" of research without breaking flow.

2.  **Context-Aware Suggestions:**
    This is the core of our "Synthesis" goal. As the user writes in Google Docs, Scrappr analyzes the text. We implemented a **TF-IDF (Term Frequency-Inverse Document Frequency)** algorithm in `algorithm.ts`. This algorithm compares the current text in the clipboard/document against the user's database of saved notes.

      * *Mechanism:* It calculates similarity scores and surfaces the most relevant past notes in the sidebar. This transforms the system from a passive storage bin into an active retrieval agent.

3.  **Voice-to-Text Integration:**
    Recognizing that ideas often come when writers are away from their keyboards, we implemented speech-to-text functionality in the mobile app. We use `React Native Voice` to capture audio, which is then transcribed and synced. We explored using LLMs to "clean up" these raw transcripts, moving from simple transcription to intelligent expansion.

### Pivot from Web App to Extension

Midway through development, we made a crucial pivot. We realized that a standalone web app required users to leave their preferred writing environment (like Google Docs). To truly solve the "fragmentation" problem, we needed to meet the user where they already were. Thus, we shifted focus to a **Chrome Browser Extension** that lives as a sidebar inside Google Docs and a **Mobile App** for capturing ideas on the fly.

## Evaluation

To validate our design, we conducted rigorous testing, starting with pilot studies and moving to formal usability tests.

### Evaluation Approach

We formulated two primary motivating questions for our evaluation:

1.  **Usability:** *What parts of the interaction are clunky or take more time than users would like?*
2.  **Utility:** *What information do people tend to put into the notes?*

**Methods:**

  * **Usability Testing:** We conducted tests with 3 users who had not participated in previous research phases. Each session lasted at least 20 minutes and covered both capturing notes (Interaction Sequence 1) and retrieving/using notes (Interaction Sequence 2).
  * **Metrics:** We measured the time taken for tasks, observed intuitive vs. forced behaviors (e.g., which buttons were clicked), and performed sentiment analysis on the notes users created.

### Findings

Our evaluation yielded a mix of validation and actionable critical feedback.

**1. Usability Challenges:**

  * **Visibility of Suggestions:** Initially, users struggled to find the suggestions because they didn't pop up intuitively. The triggers for the suggestion engine were not obvious enough.
  * **Voice Note Confusion:** Users were confused about where new voice recordings went. The extension separated "new notes" and "voice notes" in a way that fragmented the experience rather than unifying it.
  * **Overlay Issues:** The extension overlay sometimes obstructed the writing view in Google Docs, which users found "annoying".

**2. Utility Successes:**

  * **Cross-Context Capture:** Users found it "very convenient" to add notes while visiting other websites. This validated our pivot to a browser extension; users naturally used it to aggregate research links and summaries.
  * **Note Types:** We found that users primarily used notes for "information from other websites with links" or "short summaries of what they wanted to say". This confirms that Scrappr serves as a vital "staging ground" for content before it enters the final draft.

**3. Workflow Integration:**

  * **The "New Note" Tab:** This was the most frequently used feature. However, users rarely deleted notes during the session, suggesting they view Scrappr as a permanent archive rather than a temporary scratchpad.
  * **Mobile-Desktop Sync:** The ability to capture on mobile and see it on desktop was highly valued, though users requested better editing capabilities on the mobile side.

*Figure 2 below depicts the User Interface concept for the browser extension, highlighting the side-by-side integration with the writing canvas.*

**Figure 2: Scrappr Browser Extension Interface Concept**

```
+-------------------------------------------------------+
|  Google Docs Canvas                     |  Scrappr    |
|                                         |  Extension  |
|  [ User writes their draft here... ]    |             |
|  "The ocean was a vast expanse of..."   | [Suggested] |
|                                         | - Ocean     |
|  [ User highlights "expanse" ]          |   Facts     |
|           ^                             | - Note from |
|           |                             |   Nov 12    |
|    (System detects context)             |             |
|                                         | [ + New ]   |
|                                         | [ Voice ]   |
+-------------------------------------------------------+
```

## Discussion and Future Work

Our journey with Scrappr has highlighted the immense potential of **Human-AI Co-Creativity**.

### Moving from Level 2 to Level 3

Reflecting on the research paper *"Human-AI Co-Creativity: Exploring Synergies Across Levels of Creative Collaboration"*, we realized that Scrappr currently operates at **Level 2 (Task Specialist)**. It excels at retrieving existing data based on defined boundaries (TF-IDF similarity).

However, the future of Scrappr lies in moving to **Level 3 (AI Assistant)**. Instead of just saying, "Here is a note you wrote about oceans," the system should say, "Here is how your note about oceans connects to your current sentence about blue whales." We aim to implement generative features where an LLM takes the user's current text *and* the retrieved note to suggest a *new* connecting sentence. This shifts the tool from "finding old ideas" to "helping write new ones".

### Limitations

  * **Voice Interface:** The voice recording feature, while promising, faced technical hurdles. In one pilot test, the recording failed because the user didn't know which microphone was active. Clearer system status indicators are needed.
  * **Formatting Loss:** Users noted that formatting was lost when pasting notes from the extension into the doc. Preserving rich text is crucial for a seamless experience.
  * **Modal Friction:** The separation between "capturing" and "processing" still feels too distinct. We aim to automate note expansion (turning "micronotes" into full sentences) in the background to keep the user in the flow.

### Mistakes and Lessons Learned

One significant lesson was avoiding over-engineering. We initially planned to use **GraphQL** for our middleware. However, we realized during implementation that it introduced "more work with minimal gain" for our specific needs, and we successfully simplified our stack to use Firebase directly. This taught us the value of agility and cutting features that don't directly serve the core user value.

Another lesson was the importance of onboarding. Users found the UI intuitive *once they figured it out*, but the initial learning curve was steep. We realized the need for a "montage" or guided tour to help users understand the relationship between the mobile app, the extension, and the document.

## Conclusion

Scrappr began as a broad idea to "help writers." Through rigorous user research, we narrowed our focus to the specific pain point of **idea synthesis**. By building a system that lives alongside the writer—capturing "scraps" of inspiration on mobile and surfacing them intelligently on desktop—we have created a prototype that respects the writer's autonomy while augmenting their memory.

As we look to the future, we are excited to integrate deeper AI capabilities, transforming Scrappr from a passive notebook into an active co-creative partner that ensures no brilliant idea is ever lost in the shuffle.

-----

### Addendum: New Questions

Based on our evaluation, several new questions have emerged that we plan to investigate:

1.  **Long-term Retention:** Our tests were short (20 mins). How does the usage of Scrappr change over weeks? Do users accumulate "digital clutter," and how can we help them prune it?.
2.  **Algorithm Accuracy:** Is TF-IDF sufficient for creative writing, or do we need semantic vector search to truly understand the "vibe" of a story rather than just keyword matches?.
3.  **Passive vs. Active:** How can we make the suggestion engine more "passive" so it doesn't distract the user, perhaps by only appearing when the user pauses for a significant amount of time?.