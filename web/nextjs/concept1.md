## 1. Server Components
- **Rendering process:** Individual components or parts of a web page are rendered on the server, and their HTML is sent to the client's browser.
- **Initial load:** The client receives a partially rendered HTML page, with the remaining components rendered on the server as needed.
- **JavaScript role:** JavaScript is used for both initial rendering and dynamic updates, allowing for more granular control over the rendering process.
- **SEO:** SSC can be SEO-friendly, but it depends on how the components are implemented and how the server-side rendering is handled.
- Applications that require a combination of server-side and client-side rendering, dynamic content updates, or more granular control over the rendering process.
#### How are Server Components rendered:
server:
- React renders Server Components into a special data format called the React Server Component Payload (RSC Payload). 
- Next.js uses the RSC Payload and Client Component JavaScript instructions to render HTML on the server.

client side:
1. The HTML is used to immediately show a fast non-interactive preview of the oute - this is for the initial page load only.
2. The React Server Components Payload is used to reconcile the Client and Server Component trees, and update the DOM.
3. The JavaScript instructions are used to hydrate Client Components and make the application interactive.

## 2. Server Side rendering:
- **Rendering process:** The entire web page or specific components are rendered on the server before being sent to the client's browser.

- **Initial load:** The user receives a fully rendered HTML page, improving perceived performance and SEO.

- **JavaScript role:** JavaScript is primarily used for client-side interactions and dynamic updates after initial loading.

- **SEO:** SSR is generally more SEO-friendly as search engines can crawl and index the rendered HTML content.

- **Common use cases:** Applications that require fast initial page load times, SEO optimization, or complex server-side logic.

### Server Rendering Strategies:
#### 1. Static Rendering (Default)-
- **Rendering process:** The entire web page or specific components are rendered on the server during build time, resulting in static HTML files.

- The result is cached and can be pushed to a Content Delivery Network (CDN)

- **Initial load:** The user receives a pre-rendered HTML page, improving perceived performance and SEO.

- No JavaScript role

- **SEO:** Static rendering is highly SEO-friendly, as search engines can easily crawl and index the static HTML content.

- Advantages: Fast initial page load, Low maintenance, Scalability, Excellent SEO

#### 2. Dynamic Rendering:
- With Dynamic Rendering, routes are rendered for each user at request time.
- The user receives a partially rendered HTML page, with the remaining components rendered on the server as needed.
- depents on dynamic functions:
    1. cookies()
    2. headers()
    3. unstable_noStore()
    4. unstable_after():
    5. searchParams prop 
#### 3. Streaming:
- Streaming in Next.js refers to the process of sending data to the client in chunks, allowing for a more responsive user experience, especially for large datasets or long-running processes. This technique is particularly useful for applications like video streaming, real-time data updates, or large file downloads.