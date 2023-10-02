---
theme: default
background: https://massivepixel.io/wp-content/uploads/2021/10/virtual-dom-hero-header-scaled-1.jpg
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
title: RSC Slides
layout: fact
favicon: 'https://www.reactindia.io/favicons/favicon.ico'
---

# React Server Components

<div class="next">
  The next paradigm
</div>

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
.next {
 font-size: 60px;
 font-weight: 500;
 text-decoration: underline;
 margin-bottom: 20px;
}
</style>


---
transition: slide-left
layout: statement
---
# Aims
What this talk will cover:
<br/>
<v-click>

## 1. When - The history?
<br/>
<br/>
</v-click>
<v-click>

## 2. Why - The motivation?
<br/>
<br/>
</v-click>
<v-click>

## 3. How - The mechanics?
<br/>
<br/>
</v-click>


---
transition: slide-left
layout: statement
---

# When?

<v-click>
<div class="video">
<iframe width="900" height="350" src="https://www.youtube.com/embed/TQQPAU21ZUw" title="Data Fetching with React Server Components" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
</v-click>

<style>
.video {
 display: flex;
flex-direction: row;
justify-content: flex-start;
align-item:center;
}
</style>

---
transition: slide-left
layout: statement
---

# Why?
<br/>
<v-click>

## 1. Data Fetching 
<br/>
<br/>
</v-click>
<v-click>

## 2. Performance
<br/>
<br/>
</v-click>
<v-click>

## 3. User Experience
<br/>
<br/>
</v-click>

---
transition: slide-left
layout: fact
---

# "Server Components are rendered only on the Server"

---
lineNumbers: true
transition: slide-left
---
# Data fetching

```ts  {all|1|2,9|2,9,13,14|16-19}{lines:true}
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
# "Thinner bundles sent to the Client"

---
lineNumbers: true
transition: slide-left
layout: full
---
# Performance

<v-click>

```ts  {all|2|3|2,3}{lines:true}
// NoteWithMarkdown.js - Server Component
import marked from 'marked'; // 35.9K (11.2K gzipped)
import sanitizeHtml from 'sanitize-html'; // 206K (63.3K gzipped)

function NoteWithMarkdown({text}) {
    const html = sanitizeHtml(marked(text));
    return (/* render */);
}
```
</v-click>

---
transition: slide-left
layout: statement
---
# How?

<br/>
<v-click>

## 1. Server and Client have become one entity(?)
<br/>
<br/>
</v-click>
<v-click>

## 2. RSC is an architecture, a specification
<br/>
<br/>
</v-click>

---
transition: slide-left
layout: fact
---
# RSC vs 
---
transition: slide-left
layout: statement
---
# RSC vs Client-Side Components
<br/>
<v-click>

## 1. No Client Side hooks will work on Server Components
<br/>
<br/>
</v-click>
<v-click>

## 2. Client Components cannot import Server Components
<br/>
<br/>
</v-click>
<v-click>

## 3. Client and Server components can be inter-leaved
<br/>
<br/>
</v-click>

---
transition: slide-left
layout: center
---
<img src="/img_1.png" style="height: 400px;"/>

---
transition: slide-left
layout: statement
---
# RSC vs SSR
<br/>
<v-click>

## 1. HTML is not what is sent over the wire
<br/>
<br/>
</v-click>
<v-click>

## 2. Both are solutions to different problems

<br/>
<br/>
</v-click>
<v-click>

## 3. Serialization and Streaming of data is possible

<br/>
<br/>
</v-click>



---
transition: slide-left
layout: statement
---
# RSC vs (Remix/Next/Hydrogen...)
<br/>
<v-click>

## 1. Remember it is a specification!
<br/>
<br/>
</v-click>
<v-click>

## 2. The ROUTER becomes even more powerful
<br/>
<br/>
</v-click>
<v-click>

## 3. Different implementations based out of the frameworks
<br/>
<br/>
</v-click>

---
transition: slide-left
layout: statement
---
# Thank you!
## Open to audience!

<img src="/mypicture.jpeg" style="width:20%; margin-left: 40%; margin-bottom: 50px; margin-top:10px;"/>

### <carbon-logo-github/> ShaswatPrabhat | <carbon-logo-twitter/>@ShaswatPrabhat | <img width="25" height="25" style="display:inline-block; align: center;" src="https://play-lh.googleusercontent.com/G6jK9S77RN0laf9_6nhDo3AVxbRP9SgMmt8ZmQjKQ2hibn9xhOY-W5YFn_7stJD1CA=w480-h960">scavengerhere
