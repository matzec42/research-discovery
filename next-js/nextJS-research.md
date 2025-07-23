# Next.js 15 Crash Course (https://www.youtube.com/watch?v=Zq5fmkH0T78)

## Next.js Benefits
- Architecture --- functional and class-based components in React
- Next.js adds **client** and **server** components (new category ---> location)
- Automatically converts components into server components; very optimized
- You get to choose where & when content gets rendered
- Client-side rendering (browser runs & displays the JS/HTML/CSS sent by server) vs server-side rendering (renders web page on server before sending to browser, leads to instant display on client/browser)
- SSR allows for SEO (since w/ client-side rendering, the pages aren't built yet, client has to render them by running the JS code it receives). With SSR, the page is already rendered before it's received by the client
- Routing --- does not require using React Router package separately, more intuitive approach
- File-based routing
- Folder called **app** is key; e.g., a folder or file w/in app called about creates an /about route
- Full-stack framework --- w/ file-based routing, you use serverless functions to handle native backend API requests (using the **api** folder). Simply create a file called route.js in a folder and it automatically makes a route. You don't need to worry about server infrastructure.
- Handles scaling automatically (how?)
- Automatic code splitting --- when a user navigates to a different page, only code for that page is loaded (more optimized)
- Font, image, script optimizations are out of the box. HMR, bunch of other stuff

