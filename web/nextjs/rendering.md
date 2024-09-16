 # Rendering
 ## 1. Static Site Generation (SSG):
- Definition: In SSG, the **HTML pages are pre-built at build time** (ahead of time) and served to the client as static files. It **generates the entire website at the time of deployment**, creating static HTML files for each page.
- **When to Use**: Ideal for content that doesn’t change frequently, such as blogs, documentation, or marketing websites.
#### Pros:
- Very fast load times since the HTML is pre-built.
- Highly scalable since it serves static files.
- Good for SEO because the full content is available in the source at load time.
#### Cons:
- Requires a full rebuild every time content changes, which can be slow for very large sites.
- **Dynamic data** (e.g., user-specific content) needs to be fetched on the client side after the page loads.
Examples: Next.js with getStaticProps for static site generation.
#### How it works:

- At build time, the entire site is pre-rendered into HTML.
- On user request, the server serves these pre-generated HTML files

```
// In Next.js, you can use `getStaticProps` to generate static content.
export async function getStaticProps() {
  const data = await fetchData();
  return {
    props: {
      data,
    },
  };
}

```

## 2. Server-Side Rendering (SSR):
- **Definition**: With SSR, the **HTML is generated dynamically on the server per request**. When a user requests a page, the server fetches the data, renders the HTML, and sends it to the client.
- **When to Use**: Good for pages that require **dynamic content** based on the user’s request (e.g., user dashboards, personalized content).
   **Pros**:
    - Fresh, up-to-date content with every request.
    - Good for SEO since the content is available on the initial load.

    **Cons**:
    - Slower initial page loads compared to SSG since the server has to generate the HTML for each request.
    - Requires a server, which can be more resource-intensive and harder to scale.

Examples: Next.js with **getServerSideProps** for server-side rendering.
#### How it works:

On each request, the server generates the HTML page dynamically, fetching data and then sending it to the client.
```
// In Next.js, `getServerSideProps` is used to generate HTML on the server per request.
export async function getServerSideProps() {
  const data = await fetchData();
  return {
    props: {
      data,
    },
  };
}

```

## 3. Client-Side Rendering (CSR):
- **Definition**: With CSR, the HTML is an empty shell or minimal skeleton sent from the server, and the JavaScript on the client fetches the data and renders the full content. All rendering and data fetching happen on the client side.

- **When to Use**: Ideal for single-page applications (SPAs) where the primary focus is interactivity and speed after the initial load (e.g., dashboards, apps with heavy client-side logic).

Pros:
- Fast subsequent page loads after the initial load because everything is handled on the client.
- Good for highly interactive apps with dynamic content.

Cons:
- Slower initial load because the browser has to download the JavaScript and execute it before the user can see content.
- Not as SEO-friendly because search engines may not fully render content loaded by JavaScript.

Examples: Traditional React apps that use only client-side rendering (e.g., Create React App).
How it works:

**On the first load**, a minimal HTML file is sent, and JavaScript runs to populate the content.
For subsequent interactions, the app doesn't need to request new pages, making it fast after the initial load.

Next.js even allow mixing SSR, SSG, and CSR within the same project, providing flexibility to optimize each page for different requirements.