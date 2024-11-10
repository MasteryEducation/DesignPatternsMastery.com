---
linkTitle: "16.4.2 Integration Strategies"
title: "Integration Strategies for Micro Frontends: Server-Side Composition, Client-Side Composition, and More"
description: "Explore integration strategies for micro frontends, including server-side composition, client-side composition, Edge-Side Includes, Module Federation, Web Components, and more."
categories:
- Microservices
- Frontend Development
- Web Architecture
tags:
- Micro Frontends
- Integration Strategies
- Server-Side Composition
- Client-Side Composition
- Module Federation
date: 2024-10-25
type: docs
nav_weight: 1642000
---

## 16.4.2 Integration Strategies

Micro frontends are an architectural style where the frontend monolith is decomposed into smaller, more manageable pieces, each owned by different teams. This approach allows for greater flexibility, scalability, and independence in frontend development. However, integrating these micro frontends into a cohesive application presents unique challenges. In this section, we will explore various integration strategies that can be employed to effectively manage and orchestrate micro frontends.

### Use Server-Side Composition

Server-side composition is a strategy where the backend server is responsible for assembling and serving the final HTML page by aggregating micro frontends. This approach offers several advantages, particularly in terms of SEO and initial load performance.

#### Benefits of Server-Side Composition

1. **Improved SEO**: Since the server assembles the complete HTML page, search engines can easily crawl and index the content, improving the application's SEO performance.
2. **Faster Initial Load**: By delivering a fully rendered page from the server, users experience faster initial load times, as the browser does not need to fetch and assemble multiple components.
3. **Centralized Control**: The server can manage dependencies and ensure consistency across different micro frontends.

#### Implementation Example

Consider a scenario where a news website is composed of multiple micro frontends, each responsible for different sections like headlines, sports, and weather. The server-side composition can be implemented as follows:

```java
import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

public class ServerSideComposer {

    public static void main(String[] args) {
        try {
            String headlinesHtml = fetchHtml("http://microfrontend-headlines.com");
            String sportsHtml = fetchHtml("http://microfrontend-sports.com");
            String weatherHtml = fetchHtml("http://microfrontend-weather.com");

            String finalPage = "<html><body>"
                    + headlinesHtml
                    + sportsHtml
                    + weatherHtml
                    + "</body></html>";

            System.out.println(finalPage);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String fetchHtml(String urlString) throws IOException {
        URL url = new URL(urlString);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        Scanner scanner = new Scanner(url.openStream());
        StringBuilder html = new StringBuilder();
        while (scanner.hasNext()) {
            html.append(scanner.nextLine());
        }
        scanner.close();
        return html.toString();
    }
}
```

In this example, the server fetches HTML content from different micro frontends and assembles them into a single HTML page.

### Implement Client-Side Composition

Client-side composition involves loading and integrating micro frontends dynamically within the browser. This strategy provides greater flexibility and independence for frontend teams, as each micro frontend can be developed and deployed independently.

#### Benefits of Client-Side Composition

1. **Flexibility**: Teams can develop and deploy micro frontends independently, allowing for faster iterations and updates.
2. **Dynamic Loading**: Micro frontends can be loaded on demand, reducing the initial load time and improving performance.
3. **Decentralized Development**: Each team can choose their own technology stack and release cycle.

#### Implementation Example

Using JavaScript, client-side composition can be achieved by dynamically loading micro frontends:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Client-Side Composition</title>
</head>
<body>
    <div id="app"></div>
    <script>
        function loadMicroFrontend(url, containerId) {
            fetch(url)
                .then(response => response.text())
                .then(html => {
                    document.getElementById(containerId).innerHTML = html;
                })
                .catch(error => console.error('Error loading micro frontend:', error));
        }

        loadMicroFrontend('http://microfrontend-headlines.com', 'app');
        loadMicroFrontend('http://microfrontend-sports.com', 'app');
        loadMicroFrontend('http://microfrontend-weather.com', 'app');
    </script>
</body>
</html>
```

This approach allows the browser to fetch and render micro frontends dynamically, providing a seamless user experience.

### Adopt Edge-Side Includes (ESI)

Edge-Side Includes (ESI) is a templating language that allows for the dynamic assembly of web pages by including snippets from different micro frontends at the edge servers. This strategy leverages Content Delivery Networks (CDNs) to improve performance and scalability.

#### Benefits of ESI

1. **Reduced Latency**: By assembling pages at the edge, ESI reduces the latency for end-users.
2. **Scalability**: Offloading the composition task to edge servers helps scale the application efficiently.
3. **Cache Efficiency**: ESI allows for caching of individual components, improving cache hit rates.

#### Implementation Example

An ESI template might look like this:

```html
<html>
<body>
    <esi:include src="http://microfrontend-headlines.com" />
    <esi:include src="http://microfrontend-sports.com" />
    <esi:include src="http://microfrontend-weather.com" />
</body>
</html>
```

The edge server processes these ESI tags, fetching and assembling the content before delivering it to the client.

### Leverage Module Federation

Webpack 5’s Module Federation feature enables the sharing and loading of modules between different micro frontends, facilitating efficient and dynamic integration without monolithic bundling.

#### Benefits of Module Federation

1. **Dynamic Module Sharing**: Modules can be shared across different micro frontends, reducing duplication and improving load times.
2. **Independent Deployment**: Each micro frontend can be deployed independently, with shared modules updated seamlessly.
3. **Reduced Bundle Size**: By sharing common modules, the overall bundle size is reduced, improving performance.

#### Implementation Example

In a Webpack configuration, Module Federation can be set up as follows:

```javascript
// webpack.config.js
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: "app",
      remotes: {
        headlines: "headlines@http://localhost:3001/remoteEntry.js",
        sports: "sports@http://localhost:3002/remoteEntry.js",
      },
      shared: ["react", "react-dom"],
    }),
  ],
};
```

This configuration allows the `app` to consume modules from the `headlines` and `sports` micro frontends.

### Use Web Components

Web Components provide a standard for building encapsulated, reusable frontend elements that can be integrated seamlessly across micro frontends, ensuring interoperability and consistency.

#### Benefits of Web Components

1. **Encapsulation**: Web Components encapsulate styles and behaviors, preventing conflicts with other components.
2. **Reusability**: Components can be reused across different micro frontends, promoting consistency.
3. **Interoperability**: Web Components work with any JavaScript framework, providing flexibility in technology choices.

#### Implementation Example

A simple Web Component can be created using the following JavaScript:

```javascript
class MyComponent extends HTMLElement {
    constructor() {
        super();
        const shadow = this.attachShadow({ mode: 'open' });
        shadow.innerHTML = `<p>Hello, I'm a Web Component!</p>`;
    }
}

customElements.define('my-component', MyComponent);
```

This component can be used in any HTML page as `<my-component></my-component>`.

### Implement Single-SPA Framework

Single-SPA is a framework for orchestrating multiple micro frontends within a single application, managing lifecycles, routing, and inter-module communication.

#### Benefits of Single-SPA

1. **Lifecycle Management**: Single-SPA manages the lifecycle of each micro frontend, ensuring smooth transitions and updates.
2. **Routing**: It provides a unified routing mechanism, allowing micro frontends to coexist seamlessly.
3. **Inter-Module Communication**: Single-SPA facilitates communication between micro frontends, enabling data sharing and coordination.

#### Implementation Example

A basic Single-SPA setup might look like this:

```javascript
import { registerApplication, start } from "single-spa";

registerApplication({
  name: "@org/microfrontend",
  app: () => System.import("@org/microfrontend"),
  activeWhen: ["/path"],
});

start();
```

This setup registers a micro frontend to be active when the URL matches `/path`.

### Ensure Consistent State Management

Consistent state management across micro frontends is crucial for synchronizing data and user interactions. Shared state containers or global state management solutions can be employed to achieve this.

#### Strategies for State Management

1. **Shared State Containers**: Use libraries like Redux or MobX to manage state across micro frontends.
2. **Global State Management**: Implement a global state management solution to synchronize state changes.
3. **Event Bus**: Use an event bus for communication between micro frontends, ensuring consistent state updates.

### Optimize Performance and Load Times

Optimizing the performance and load times of integrated micro frontends is essential for ensuring a smooth and responsive user experience.

#### Performance Optimization Strategies

1. **Lazy Loading**: Load micro frontends on demand to reduce initial load times.
2. **Code Splitting**: Split code into smaller chunks to improve load times and performance.
3. **Efficient Asset Management**: Use caching and CDNs to manage assets efficiently.

### Conclusion

Integrating micro frontends requires careful consideration of various strategies to ensure a cohesive and performant application. By leveraging server-side and client-side composition, Edge-Side Includes, Module Federation, Web Components, and frameworks like Single-SPA, developers can create scalable and maintainable frontend architectures. Consistent state management and performance optimization further enhance the user experience, making micro frontends a powerful approach for modern web applications.

## Quiz Time!

{{< quizdown >}}

### What is a key benefit of server-side composition for micro frontends?

- [x] Improved SEO
- [ ] Greater flexibility for frontend teams
- [ ] Dynamic loading of components
- [ ] Decentralized development

> **Explanation:** Server-side composition improves SEO by delivering fully rendered HTML pages that search engines can easily crawl and index.

### Which strategy allows for dynamic assembly of web pages at edge servers?

- [ ] Server-Side Composition
- [ ] Client-Side Composition
- [x] Edge-Side Includes (ESI)
- [ ] Module Federation

> **Explanation:** Edge-Side Includes (ESI) is a templating language that allows for dynamic assembly of web pages at edge servers.

### What is a primary advantage of client-side composition?

- [ ] Improved SEO
- [x] Greater flexibility and independence for frontend teams
- [ ] Faster initial load times
- [ ] Centralized control

> **Explanation:** Client-side composition provides greater flexibility and independence for frontend teams, allowing them to develop and deploy micro frontends independently.

### What feature of Webpack 5 facilitates the sharing of modules between micro frontends?

- [ ] Lazy Loading
- [ ] Code Splitting
- [x] Module Federation
- [ ] Web Components

> **Explanation:** Webpack 5's Module Federation feature enables the sharing and loading of modules between different micro frontends.

### How do Web Components benefit micro frontend integration?

- [x] They provide encapsulation and reusability
- [ ] They improve SEO
- [ ] They enable server-side composition
- [ ] They manage lifecycles and routing

> **Explanation:** Web Components provide encapsulation and reusability, allowing for consistent and interoperable frontend elements across micro frontends.

### Which framework is used for orchestrating multiple micro frontends within a single application?

- [ ] Webpack
- [ ] React
- [x] Single-SPA
- [ ] Angular

> **Explanation:** Single-SPA is a framework for orchestrating multiple micro frontends within a single application, managing lifecycles, routing, and inter-module communication.

### What is a strategy for ensuring consistent state management across micro frontends?

- [ ] Server-Side Composition
- [ ] Edge-Side Includes
- [x] Shared State Containers
- [ ] Lazy Loading

> **Explanation:** Shared state containers, such as Redux or MobX, can be used to manage state consistently across micro frontends.

### Which performance optimization technique involves loading components on demand?

- [x] Lazy Loading
- [ ] Code Splitting
- [ ] Efficient Asset Management
- [ ] Module Federation

> **Explanation:** Lazy loading involves loading components on demand, reducing initial load times and improving performance.

### What is the role of an event bus in micro frontend integration?

- [ ] To improve SEO
- [ ] To manage lifecycles
- [x] To facilitate communication between micro frontends
- [ ] To dynamically load components

> **Explanation:** An event bus facilitates communication between micro frontends, ensuring consistent state updates and coordination.

### True or False: Module Federation reduces the bundle size by sharing common modules across micro frontends.

- [x] True
- [ ] False

> **Explanation:** Module Federation reduces the bundle size by allowing micro frontends to share common modules, improving performance and reducing duplication.

{{< /quizdown >}}
