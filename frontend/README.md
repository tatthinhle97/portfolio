# Installation

https://nextjs.org/docs/app/getting-started/installation

## Create with the CLI

https://nextjs.org/docs/app/getting-started/installation#create-with-the-cli

```
npx create-next-app@latest
```

# Run the application

```bash
$ npm run dev
```

# NextJS

## Project structure

- `/app`: Contains all the routes, components, and logic for the application
- `/app/lib`: Contains reusable utility functions and data fetching functions.
- `/app/ui`: Contains all the UI components for your application, such as cards, tables, and forms.
- `/public`: Contains all the static assets for your application, such as images.
- Config files: Created and pre-configured when you start a new project using create-next-app.

## Custom styling

❓ What is one benefit of using CSS modules?

📝 Provide a way to make CSS classes locally scoped to components by default, reducing the risk of styling conflicts.

Example: Create a new file called `home.module.css`

```css
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

Usage:

```tsx
import styles from '@/app/ui/home.module.css';
 
export default function Page() {
  return <div className={styles.shape} />
}
```

## Fonts

Next.js automatically optimizes fonts in the application when you use the next/font module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are **no additional network requests for fonts** which would impact performance.

### Add a font

*Note: Visit the Google Fonts website to see what options are available.*

`/app/ui/fonts.ts`

```ts
import { Inter, Lusitana } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });
```

`/app/layout.tsx`

```tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

## Images (`<Image>`)

The <Image> Component comes with automatic image optimization such as:

- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like WebP and AVIF, when the browser supports it.

It's good practice to set the width and height of your images to avoid layout shift, these should be an aspect ratio identical to the source image. These values are not the size the image is rendered, but instead the size of the actual image file used to understand the aspect ratio.

Example: setting an image ratio on desktop and mobile devices:

```tsx
...
import Image from 'next/image';
 
export default function Page() {
  return (
    // ...
    <div>
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
      <Image
        src="/hero-mobile.png"
        width={560}
        height={620}
        className="block md:hidden"
        alt="Screenshot of the dashboard project showing mobile version"
      />
    </div>
    //...
  );
}
```

## Nested routes

To create a nested route, nest folders inside each other and add `page.tsx` files inside them.

Example: `/app/dashboard/page.tsx` is associated with the `/dashboard` path.

## Layout

In Next.js, you can use a special `layout.tsx` file to create UI that is shared between multiple pages.

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called **partial rendering** which preserves client-side React state in the layout when transitioning between pages.

## Navigation (`<Link>`)

In Next.js, use the `<Link />` Component to link between pages in the application. `<Link>` allows **client-side navigation** with JavaScript.

Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

### Active link

```tsx
...
import { usePathname } from 'next/navigation';
import clsx from 'clsx';
 
export default function NavLinks() {
  const pathname = usePathname();
 
  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
          <Link
            key={link.name}
            href={link.href}
            className={clsx(
              {
                'bg-sky-100 text-blue-600': pathname === link.href,
              },
            )}
          >
            ...
          </Link>
        );
      })}
    </>
  );
}
```

# Concepts

## Web UI

### Cumulative Layout Shift

Cumulative Layout Shift (CLS) is primarily caused by the **unexpected movement of page elements** as content loads, particularly from images, ads, iframes, and dynamically injected content without predefined dimensions, as well as web fonts and late-running third-party scripts. These shifts create an unstable user experience by making it difficult to **find content** or **click the correct elements**.

Common causes:
- **Images**, embeds, and iframes without dimensions.
- Dynamically injected content.
- **Fonts**.
- Late-running third-party scripts.

## Data 

### Placeholder data

Placeholder data is temporary, often fake or incomplete data used to fill a space where real data will eventually go, serving as a stand-in until the final content is available or to allow for the development and testing of systems without relying on live data.

