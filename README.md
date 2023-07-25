### Intro to HydrogenJS

Fast-track storefront build with Hydrogen, React-based headless commerce stack, now built on Remix.
Deploy for free on Oxygen, our global hosting solution.

---

### Why HydrogenJS?

With the hardest parts of the storefront built in, ready to use, or easily integrated with Shopify, and significant performance advancements unlocked by Remix, the Hydrogen stack makes building storefronts fun.

__[+] Starter templates__ <br />
Fully built-out Demo Store template includes purchase journey and Hello World template offers minimal opinions with optional TypeScript support

__[+] Components and hooks__ <br />
Mapped directly to the Storefront API, Hydrogen gives you components and hooks like customer accounts, product forms, search, pagination, and i18n—all pre-built to accelerate development

__[+] SEO ready__ <br />
Includes pre-built SEO optimizations like an auto-generated sitemap, metadata values for every page, robust product feed support, as well as improved crawlability for all engines

__[+] Streaming server-side rendering with Suspense__ <br />
Streaming server-side rendering with Suspense allows page components to progressively load based on priority without sacrificing page interactivity

__[+] Oxygen deployment__ <br />
Deployed at the edge on Oxygen, Hydrogen storefronts render static and dynamic content faster for performant storefronts worldwide

__[+] TypeScript__ <br />
Robust type support for the Storefront API and all Hydrogen components, with IntelliSense editing support and VS Code tooling, so writing code is more intuitive with fewer errors

__[+] Powered by Remix__ <br />
Unlock advancements in developer experience and performance, like optimistic UI, nested routes, progressive enhancement, and more

__[+] Built-in CSS Support__ <br />
Built-in support for CSS strategies like Tailwind, CSS Modules, and Vanilla Extract; a native feature with Remix.

---

### Oxygen deployment

__[+] Analytics__ <br />
Performance insights, analytics, and logs for every deployment

__[+] GitHub connectivity__ <br />
Oxygen connects directly to GitHub and uses GitHub Actions to automatically deploy commits, so deployment is as simple as a git push

__[+] Preview deployments__ <br />
Every commit gets its own preview deployment, defaulting to private but easily made public

__[+] Custom environments__ <br />
Tied directly to a branch, custom environments get their own environment variables and a static URL

---

### What is RemixJS?

Most web apps fetch inside of components, creating request waterfalls which results in slower loads. While Remix loads data in parallel on the server and sends a fully formed HTML document which results which makes loading faster.

---

### Folder Structure

```
🌳 hello-world
├── 📁 app
|  ├── 📁 components
|  ├── 📁 routes
|  ├── 📁 styles
|  ├── 📄 entry.client.tsx
|  ├── 📄 entry.server.tsx
|  └── 📄 root.tsx
├── 📁 dist
|  ├── 📁 client
|  └── 📁 worker
├── 📁 public
|  └── 📄 favicon.svg
├── 📄 .env
├── 📄 package.json
├── 📄 README.md
├── 📄 remix.config.ts
└── 📄 server.ts
```

#### /server.ts
The `server.ts` file is the main application entry point. After `handleServer()` is called within `server.ts`, you can manipulate the Response object before it's returned to the client. Here you can add headers or other custom response logic.

#### /.env
The `.env` file is where you define environment variables for local development. These variables aren't used in production.

#### /public
The `/public` directory is where static assets are stored. Everything within the directory is uploaded to Shopify's CDN on the deploy to Oxygen. It's the recommended location for images and other static files that don't change.

#### /app/root.tsx
The `root.tsx` file is the root route of any Remix application. Setup the base document here, including any custom `<link>`, `<meta>`, or `<script>` tags that are global to the application. The root route is also where a Remix loader should be defined for any global data that's necessary for the entire application. For example, the storefront name, description, and cart should be queried from here.

#### /app/routes
The `/app/routes` directory is where all routes are defined in a Remix application.


#### /app/entry.client.tsx
Remix uses `/app/entry.client.tsx` as the entry point for the browser bundle. This module gives you full control over the "hydrate" step after JavaScript loads into the document

#### /app/entry.server.tsx
Remix uses `/app/entry.server.tsx` to generate the HTTP response when rendering on the server. The default export of this module is a function that lets you create the response, including HTTP status, headers, and HTML, giving you full control over the way the markup is generated and sent to the client.

#### /app/components
By convention, Hydrogen uses the `/app/components` directory to store reusable React components in the application. This is necessary because all tsx files within `/app/routes` correspond to the application's URL paths.

#### /dist
The `dist` folder is where both the server and client artifacts are generated. It should be added to your .gitignore file.

#### /remix.config.ts
The `remix.config.ts` file has a few build and development configuration options, but does not actually run on your server.

---

### Documentation / Community Support

HydrogenJS is having a fairly good documentation for what has been added. But, you have to dig deeper into the documentation to get your exact answer.

There is no mention to disable sourcemap from the build. Which makes app of the website vulnerable to easy copy paste.

----

### Server State > Client State

Most React applications rely heavily on component state and context to provide data throughout the app tree. For example, on a product detail page there might be a product options selector. A simple implementation is to use component state to track what product options are selected:

```jsx
export function ProductOptions({ availableOptions }) {
  const [option, setOption] = useState(getDefaultOption);

  return (
    <ListBox
      availableOptions={availableOptions}
      selectedOption={option}
    />
  );
}
```

The problem with this code is that it quickly gets complicated. The selected product option and associated product variant should be tied to the URL. This is important so that the user can select a specific variant and then bookmark or share the URL without losing the selected state. So when a selected option is changed, the URL needs to be updated and getDefaultOption needs to be aware of the current URL state. Additionally, a context provider is often added so that any component in the tree can get access to product options.

However, Remix makes synchronizing URL state with app state extremely easy. Consider this simplified Remix code:

```jsx
export async function loader({ request }) {
  const product = await queryProductVariant(request.url);
  return json({ product });
}
    
export default function ProductDetailPage() {
  const { product } = useLoaderData();

  return (
    <>
      <ProductDetails product={product} />
        {product.variants.map((variant) => (
          <Link to={`/products/${variant.id}`}>
            {variant.name}
          </Link>
        ))}
    </>
  );
}
```

---

### loader
One of the primary features of Remix is simplifying interactions with the server to get data into components.

```tsx
import { json } from "@remix-run/node"; // or cloudflare/deno
import { useLoaderData } from "@remix-run/react";

export async function loader() {
  const res = await fetch("https://api.github.com/gists");
  return json(await res.json());
}

export default function GistsRoute() {
  const gists = useLoaderData<typeof loader>();
  return (
    <ul>
      {gists.map((gist) => (
        <li key={gist.id}>
          <a href={gist.html_url}>{gist.id}</a>
        </li>
      ))}
    </ul>
  );
}
```

With React streaming set up, now you can start adding Await usage for your slow data requests where you'd rather render a fallback UI. 

```jsx
import type { LoaderArgs } from "@remix-run/node"; // or cloudflare/deno
import { defer } from "@remix-run/node"; // or cloudflare/deno
import { Await, useLoaderData } from "@remix-run/react";
import { Suspense } from "react";

import { getPackageLocation } from "~/models/packages";

export function loader({ params }: LoaderArgs) {
  const packageLocationPromise = getPackageLocation(
    params.packageId
  );

  return defer({
    packageLocation: packageLocationPromise,
  });
}

export default function PackageRoute() {
  const data = useLoaderData<typeof loader>();

  return (
    <main>
      <h1>Let's locate your package</h1>
      <Suspense
        fallback={<p>Loading package location...</p>}
      >
        <Await
          resolve={data.packageLocation}
          errorElement={
            <p>Error loading package location!</p>
          }
        >
          {(packageLocation) => (
            <p>
              Your package is at {packageLocation.latitude}{" "}
              lat and {packageLocation.longitude} long.
            </p>
          )}
        </Await>
      </Suspense>
    </main>
  );
}

```

---

### action

Like loader, action is a server-only function to handle data mutations and other actions. If a non-GET request is made to your route (POST, PUT, PATCH, DELETE) then the action is called before the loaders.

```tsx
import type { ActionArgs } from "@remix-run/node"; // or cloudflare/deno
import { json, redirect } from "@remix-run/node"; // or cloudflare/deno
import { Form } from "@remix-run/react";

import { TodoList } from "~/components/TodoList";
import { fakeCreateTodo, fakeGetTodos } from "~/utils/db";

export async function loader() {
  return json(await fakeGetTodos());
}

export async function action({ request }: ActionArgs) {
  const body = await request.formData();
  const todo = await fakeCreateTodo({
    title: body.get("title"),
  });
  return redirect(`/todos/${todo.id}`);
}

export default function Todos() {
  const data = useLoaderData<typeof loader>();
  return (
    <div>
      <TodoList todos={data} />
      <Form method="post">
        <input type="text" name="title" />
        <button type="submit">Create Todo</button>
      </Form>
    </div>
  );
}
```

---

### meta

This component renders all of the `<meta>` tags created by your route module `meta` export. These tags are important for things like search engine optimization (SEO). They can also be used by social media sites to display rich previews of your app.

```tsx
import { Meta } from "@remix-run/react";

export const loader = async () => {
  return json({ name: "Codal" });
};

export const meta: MetaFunction<typeof loader> = ({ data }) => {
  return {
    title: `Welcome ${data.name}`,
    description: "This becomes the nice preview on search results.",
  };
};


export default function Root() {
  return (
    <html>
      <head>
        <Meta />
      </head>
      <body></body>
    </html>
  );
}
```

---

### CatchBoundary

A `CatchBoundary` is a React component that renders whenever an action or loader throws a `Response`.

```tsx
import { useCatch } from "@remix-run/react";

export function CatchBoundary() {
  const caught = useCatch();

  return (
    <div>
      <h1>Caught</h1>
      <p>Status: {caught.status}</p>
      <pre>
        <code>{JSON.stringify(caught.data, null, 2)}</code>
      </pre>
    </div>
  );
}
```

---

### ErrorBoundary

An `ErrorBoundary` is a React component that renders whenever there is an error anywhere on the route, either during rendering or during data loading. We use the word "error" to mean an uncaught exception; something you didn't anticipate happening. You can intentionally throw a `Response` to render the `CatchBoundary`, but everything else that is thrown is handled by the `ErrorBoundary`.

```tsx
export function ErrorBoundary({ error }) {
  return (
    <div>
      <h1>Error</h1>
      <p>{error.message}</p>
      <p>The stack trace is:</p>
      <pre>{error.stack}</pre>
    </div>
  );
}
```

For example:
```
routes
├── sales
│   ├── invoices
│   │   └── $invoiceId.tsx
│   └── invoices.tsx
└── sales.tsx
```

If `$invoiceId.tsx` exports an `ErrorBoundary` and an error is thrown in its component, loader, or action, the rest of the app renders normally and only the invoice section of the page renders the error.

![ErrorBoundary](https://remix.run/docs-images/error-boundary.png)

---

### useFetcher

Sometimes you want to call a loader outside of navigation, or call an action (and get the routes to reload) but you don't want the URL to change. Many interactions with the server aren't navigation events. This hook lets you plug your UI into your actions and loaders without navigating.

```tsx
import { useFetcher } from "@remix-run/react";

const CodalRemix = () => {
  const fetcher = useFetcher();

  // trigger the fetch with these
  <fetcher.Form {...formOptions} />;

  useEffect(() => {
    fetcher.submit(data, options);
    fetcher.load(href);
  }, [fetcher]);

  // build UI with these
  fetcher.state;
  fetcher.data;
  fetcher.submit;
}
```

---

### Demo

To get started, you'll use the Shopify CLI. In your Terminal, run the following command:

```bash
yarn create @shopify/hydrogen
```

Start the development server

```bash
npm run dev
```

You can reach the development server at `http://localhost:3000/`.