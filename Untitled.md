2025-07-19T07:47:43.933Z [error] Error fetching recently played tracks: Error: Bad OAuth request (wrong consumer key, bad nonce, expired timestamp...). Unfortunately, re-authenticating the user won't help here. Body: Check settings on developer.spotify.com/dashboard, the user may not be registered.
    at L.validateResponse (.next/server/app/api/recently-played/route.js:1:20879)
    at async J.makeRequest (.next/server/app/api/recently-played/route.js:1:24994)
    at async k.getRequest (.next/server/app/api/recently-played/route.js:1:3308)
    at async Y (.next/server/app/api/recently-played/route.js:1:26903)
2025-07-19T07:47:43.933Z [error] Error message: Bad OAuth request (wrong consumer key, bad nonce, expired timestamp...). Unfortunately, re-authenticating the user won't help here. Body: Check settings on developer.spotify.com/dashboard, the user may not be registered.
2025-07-19T07:47:43.933Z [error] Error stack: Error: Bad OAuth request (wrong consumer key, bad nonce, expired timestamp...). Unfortunately, re-authenticating the user won't help here. Body: Check settings on developer.spotify.com/dashboard, the user may not be registered.
    at L.validateResponse (/var/task/.next/server/app/api/recently-played/route.js:1:20879)
    at process.processTicksAndRejections (node:internal/process/task_queues:105:5)
    at async J.makeRequest (/var/task/.next/server/app/api/recently-played/route.js:1:24994)
    at async k.getRequest (/var/task/.next/server/app/api/recently-played/route.js:1:3308)
    at async Y (/var/task/.next/server/app/api/recently-played/route.js:1:26903)
    at async to.do (/var/task/node_modules/next/dist/compiled/next-server/app-route.runtime.prod.js:18:18605)
    at async to.handle (/var/task/node_modules/next/dist/compiled/next-server/app-route.runtime.prod.js:18:23632)
    at async eg (/var/task/node_modules/next/dist/compiled/next-server/server.runtime.prod.js:31:28400)
    at async n2.renderToResponseWithComponentsImpl (/var/task/node_modules/next/dist/compiled/next-server/server.runtime.prod.js:32:1849)
    at async n2.renderPageComponent (/var/task/node_modules/next/dist/compiled/next-server/server.runtime.prod.js:32:7200)