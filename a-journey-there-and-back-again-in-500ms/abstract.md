# A developer's tale, there and back again in under 500ms

RFC-2616 is the medium upon which every single element of what a web developer implements stands. You may be a HTML Rockstar and CSS Ninja and DevOps Master or a PHP Superhero. It does not matter which your work is impacted by this specification.

During this talk we plot the path of a single lowly byte of information and how our decisions can impact that journey and take us closer or further from the goal of a sub 500ms response to the browser.

This will be distilled into a small set of questions to ask and guiding principles to follow while building websites that will fly. This will enable you to make the appropriate decisions to avoid performance problems or understand the areas of investigation to resolve existing performance bottlenecks.

# Outline

* Initiate request of remote system
  * Type URL in the browser
  * Click link
  * Issue curl request
* DNS Resolution
  * Multicast DNS
  * TLD impact
* Protocol
* HTTP
  * HTTP 1.1
    * REST
    * XML-RPC
  * HTTP 2.0 (Is the new model of interest / relevant yet e.g. SPDY)
  * SOAP?
* Traffic routing
  * Loadbalancing
    * Round robin
    * Sticky sessions
  * HTTP/HTTPs
    *SSL off-load
* Server
  * SSL
  * Proxy cache server
    * Varnish
    * Apache traffic server
  * HTTP Server
    * Apache
    * Nginx
  * State | Stateless
    * Cookies
    * Session Cookies
* Asset / Page / Resource delivery
  * Performance budget
  * KB limit (mobile vs desktop)
  * HTTP request limit
  * Cookie size / qty
  * kb down the wire (mobile vs desktop)
  * Images
  * Sprite sheets
  * HTML
  * Javascript
  * Merging files anti-pattern
  * CDNs
  * RWD
  * RESS
* HTTP Cache
  * Cache
  * Expires Vs ETags
  * Proxy Cache
* CloudFlare
  * Proxy Cache
  * Optimisation
  * CDN
  * Rail Gun
* Collective Team Responsibility for Performance
* When to consider Performance in the project delivery cycle




## 4 Simple rules of performance

Review these and try and condense everything above into a small set of simple rules that will guide you to performance in any area.

1. Minimize the Payload https://developers.google.com/speed/docs/best-practices/payload (if possible also use a responsive image solution such as Cloudinary, to reduce the image of image bandwidth especially on responsive sites; can adapt image size served based on viewport width)

1. Distribute the Payload - If a library (e.g. Angular, JQuery, Twitter Bootstrap) is available on a public CDN, then use it (if you have to develop in a disconnected environment you can use consider locally hosting)

1. Use Google PageSpeed Insights / Audits for Chrome/Sitespeed.io and eliminate any item reported worse than ‘L’ (Low), and eliminate on-page ‘L’s. One of the most important I feel here is caching and using caching headers.

1. Rendering performance on mobile and desktop should be 60fps. 30FPS is also fine on mobile as long as it is a consistent frame rate. Also considerations for battery life on mobile.
