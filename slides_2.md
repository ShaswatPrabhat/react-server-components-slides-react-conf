---
theme: default
background: https://massivepixel.io/wp-content/uploads/2021/10/virtual-dom-hero-header-scaled-1.jpg
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
title: React Server Components
layout: fact
favicon: 'https://www.reactindia.io/favicons/favicon.ico'
---

# React Server Components

<span class="author">
    Shaswat Prabhat | <carbon-logo-github/> ShaswatPrabhat | <carbon-logo-twitter/>@ShaswatPrabhat | <img width="25" height="25" style="display:inline-block; align: center;" src="https://play-lh.googleusercontent.com/G6jK9S77RN0laf9_6nhDo3AVxbRP9SgMmt8ZmQjKQ2hibn9xhOY-W5YFn_7stJD1CA=w480-h960">scavengerhere
</span>


<style>
p{
  font-size:24px;
}
.author {
  font-size: 20px;
  margin-top: 10%;
}
</style>


---
layout: two-cols
transition: slide-left
---

# Aims
What this talk will cover:
<br/>
<v-click>

### 1. Mental Model 
<br/>
<br/>
</v-click>
<v-click>

### 2. Motivation
<br/>
<br/>
</v-click>
<v-click>

### 3. Re-understand "Server"
<br/>
<br/>
</v-click>

::right::

<v-click>

# Anti-Aims
What this talk will not cover:
<br/>
</v-click>
<v-click>

### 1. Comparison of landscape
<br/>
<br/>
</v-click>
<v-click>

### 2. Infrastructure and Setup
<br/>
<br/>
</v-click>
<v-click>

### 3. Framework implementations
<br/>
<br/>
</v-click>

---
transition: slide-left
---

# A super quick vocabulary check
  <br/> 

* <carbon-logo-react /> React - A client-side library for components based Web Application 
  <br/>  <br/>
* <carbon-connection-two-way /> State - Intelligent variables
  <br/>  <br/>
* <carbon-laptop /> CSR - Client side Rendering
    <br/>  <br/>
* <carbon-bare-metal-server /> SSR - Server side Rendering

---
transition: slide-left
layout: fact
---

# Network Chasm and attention deficit
Lesser time available to grab user's attention

---
transition: slide-left
---

# Path to Server Components - I
<img 
height="640"
width="640"
src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fthinking-in-server-components.png&w=3840&q=75&dpl=dpl_9ban2Vw5rf63ZAqeHHQT5v4nYHuz">
* Page divided into Interactive and non-interactive components
* What can we do after having segregated this?
<p style="font-size:12px; color: aqua"><mdi-alert-box /> Several ideas span from the above - including Micro front-ends, Islands Architecture etc</p>
---
transition: slide-left
---

# Path to Server Components - II
<img height="600"
width="600"
src="https://s42821.pcdn.co/wp-content/uploads/2022/07/Image-AWS-Edge-Network@2x.png">
* Rise of "Edge runtimes" - deploy closer to the user
* The network chasm between Client and Server is decreasing
<p style="font-size:12px; color: aqua"><mdi-alert-box /> Almost all layers of the OSI stack are becoming faster and scalabale </p>
---
transition: slide-left
---

# Path to Server Components - III
  <br/>

* <mdi-connection /> The database, services and browsers are quickly becoming more connected
  <br/>  <br/>
* <mdi-server-remove /> While Client-Server Architecture is still relevant, we might need to <u>unlearn</u> some of the concepts
  <br/>  <br/>
* <mdi-speedometer /> Our applications need to be ready to deliver content at scale and at speed
  <br/>  <br/>
---
transition: slide-left
layout: statement
---

# <mdi-transit-connection-variant /> All of these ideas converge into RSC
---
transition: slide-left
layout: fact
---

# Parsing the RFC
Some phrases from the Request For Comments and their intent

---
transition: slide-left
layout: statement
---
# "Spanning the client and the server"

---
transition: slide-left
layout: statement
---
# "Incrementally Stream rendered data"

---
transition: slide-left
layout: statement
---
# "...additional endpoints to power their UI"

<br/>

# "Easier to avoid <u>client-server</u> Network waterfall"

---
transition: slide-left
layout: statement
---
# "Top level awaits"
---
transition: slide-left
layout: statement
---
# A quick example <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f525/512.gif" alt="🔥" width="70" height="70" style="display:inline-block; align: center; margin-bottom: 20px">
---
lineNumbers: true
transition: slide-left
---
```ts  {all|2|4|9|16-19|16-19,4|16-19,4,7}{lines:true}
// Note.js - Server Component
import db from 'db';
// We import from NoteEditor.js - a Client Component.
import NoteEditor from 'NoteEditor';

async function Note(props) {
    const {id, isEditing} = props;
// Can directly access server data sources during render, e.g. databases
    const note = await db.posts.get(id);

    return (
        <div>
            <h1>{note.title}</h1>
        <section>{note.body}</section>
    {/* Dynamically render the editor only if necessary */}
    {isEditing
        ? <NoteEditor note={note} />
    : null
    }
    </div>
);
}
```
---
transition: slide-left
layout: statement
---
# Thinner bundles sent to the Client

---
lineNumbers: true
transition: slide-left
layout: center
---
```ts  {all}{lines:true}
import marked from 'marked'; // 35.9K (11.2K gzipped)
import sanitizeHtml from 'sanitize-html'; // 206K (63.3K gzipped)

function NoteWithMarkdown({text}) {
    const html = sanitizeHtml(marked(text));
    return (/* render */);
}
```

---
transition: slide-left
layout: statement
---
# Rendering of a Server Component

---
transition: slide-left
layout: default
---
# Rendering of a Server Component - I
  <br/>  <br/>
<v-click>

1. Browser triggers a request

</v-click>
<v-click>
<br/>  <br/>

2. Server Component begins to render on the server

</v-click>
<v-click>
<br/>  <br/>

3. If it contains more Server components these get rendered to pure `html`: `<div/>`s `<span/>`s etc.

</v-click>
---
transition: slide-left
layout: statement
---
# What about the Client components?

---
transition: slide-left
layout: default
---
# Rendering of a Server Component - II
  <br/>  <br/>
<v-click>

4. Client components  need to be rendered on the browser

</v-click>
<v-click>
<br/>  <br/>

5. These are replaced by <u>placeholders</u> on the Server

</v-click>
<v-click>
<br/>  <br/>

6. And then sent to the Browser

</v-click>
---
transition: slide-left
layout: statement
---
# And then sent to the Browser as pure HTML right?

---
transition: slide-left
layout: default
---
# Rendering of a Server Component - III
  <br/>  <br/>
<v-click>

7. Over the wire we get a <u>JSON stream</u> that React understands

</v-click>
<v-click>
<br/>  <br/>

8. Browser replaces the placeholder with Client rendered components

</v-click>
<v-click>

<br/>  <br/>

9. Page is ready with Client + Server Components <img src="https://fonts.gstatic.com/s/e/notoemoji/latest/1f525/512.gif" alt="🔥" width="60" height="60" style="display:inline-block; align: center; margin-bottom: 20px">

</v-click>

<v-click>

<p style="font-size:12px; color: aqua"><mdi-alert-box /> There are active parts played the Server, the Bundler and Caching in this flow</p>

</v-click>

---
transition: slide-left
layout: statement
---
# Puzzles

---
transition: slide-left
layout: default
---
# Open Questions ( and some answers )...

<v-click>

* Should we migrate all our apps to RSC?

</v-click><v-click>

* How do we handle routing?

</v-click>

<v-click>

* How much of the implementation is dependent on frameworks?

</v-click>
<v-click>

* Will co-locating code add more complexity and security issues?

</v-click>

<v-click>

* What role will bundlers play?

</v-click>
<v-click>

* What about SSR?

</v-click>

---
transition: slide-left
layout: statement
---
# Open to audience!
<br/> <br/>
---
transition: slide-left
layout: statement
---
# Thank you!

<img src="/mypicture.jpeg" style="width:20%; margin-left: 40%; margin-bottom: 40px"/>

#### Shaswat Prabhat | <carbon-logo-github/> ShaswatPrabhat | <carbon-logo-twitter/>@ShaswatPrabhat | <img width="25" height="25" style="display:inline-block; align: center;" src="https://play-lh.googleusercontent.com/G6jK9S77RN0laf9_6nhDo3AVxbRP9SgMmt8ZmQjKQ2hibn9xhOY-W5YFn_7stJD1CA=w480-h960">scavengerhere
