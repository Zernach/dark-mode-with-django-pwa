# Dark Mode Switcher

``DarkMode switcher using Django``   ``Implemented Progressive WebApp``
``Service-Worker``

## ``Django Progressive Web App``
- ``Add on Home screen feature`` <br>
- ``Offline caching fearure`` <br>
- ``Notification Feature`` <br>
- ``Web Manifest Feature `` <br>
- ``Django staticfile handling`` <br>


## ``Django App/PWA``
- django-admin startproject django_pwa <br>
``Start App`` ``->`` ``Go to project folder``
- python manage.py startapp app <br>
``Runnig server`` ``->`` ``Go to project folder``
- python manage.py runserver

## ``Django staticfiles``
```staticfiles
"Adding staticfiles to Django project"

Go to settings.py file and add this line,
----------------------------------------
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'assets'),
]
---------------------------------------
where assets is a folder which contain the 
static files such as css,bootstrap,js and 
image files.
```
## ``Work with progressive webapp``
- Add manifest.json file in staticfiles
```manifest
{
  "name": "Django Progressive Web App",
  "short_name": "PWA",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#FFE9D2",
  "theme_color": "#FFE1C4",
  "orientation": "portrait-primary",
  "icons": [
    {
      "src": "img/icons/icon-72x72.png",
      "type": "image/png",
      "sizes": "72x72"
    },
    {
      "src": "img/icons/icon-96x96.png",
      "type": "image/png",
      "sizes": "96x96"
    },
    {
      "src": "img/icons/icon-128x128.png",
      "type": "image/png",
      "sizes": "128x128"
    },
    {
      "src": "img/icons/icon-144x144.png",
      "type": "image/png",
      "sizes": "144x144"
    },
    {
      "src": "img/icons/icon-152x152.png",
      "type": "image/png",
      "sizes": "152x152"
    },
    {
      "src": "img/icons/icon-192x192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "img/icons/icon-384x384.png",
      "type": "image/png",
      "sizes": "384x384"
    },
    {
      "src": "img/icons/icon-512x512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ]
}
```








## ``Service Worker Javascript``
```javascript
const staticCacheName = "static-cache" + '-' + new Date().getDate() + ':' + new Date().getHours() + ':' + new Date().getSeconds();
const filesToCache = [
    '/',
    '/offline',
    '/assets/manifest.json',
    '/assets/img/icons/icon-96x96.png'
];

// Cache on install
self.addEventListener("install", event => {
    console.log("Installing service worker...");
    event.waitUntil(
        caches.open(staticCacheName)
            .then(cache => {
                return cache.addAll(filesToCache);
            })
    )
    console.log('Service worker installed and cached files');
    this.skipWaiting();
});

// Clear cache on activate
self.addEventListener('activate', event => {
    console.log('Service worker is activating...')
    event.waitUntil(
        caches.keys().then(cacheNames => {
            return Promise.all(
                cacheNames
                    .filter(cacheName => (cacheName.startsWith("static-cache")))
                    .filter(cacheName => (cacheName !== staticCacheName))
                    .map(cacheName => caches.delete(cacheName))
            );
        })
    );

    console.log('service worker activated')
});

// Serve from Cache
self.addEventListener("fetch", event => {
    console.log('fetch event fired')
    event.respondWith(
        caches.match(event.request)
            .then(response => {
                return response || fetch(event.request);
            })
            .catch(() => {
                return caches.match('offline');
            })
    )
});
```