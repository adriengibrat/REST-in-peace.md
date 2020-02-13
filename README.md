---
marp: true
theme: uncover
headingDivider: 3
html: true
style: |
  li { list-style: none; }
  li p, .mermaid { display: inline-block; vertical-align: middle; }
  li p + .mermaid { max-width: 90%; }
  .mermaid svg { max-width: 100%; max-height: 40vh; }
  .mermaid .edgeLabel:not(:empty) { padding: 0 .25em; }
  small { font-size: .5em; color: grey; }
footer: '![R&D footer](https://adriengibrat.github.io/Password.md/theme/img/footer.png)'
---

# REST in ☮️

👑 Long live to the Web 👑

<small>by [Adrien Gibrat](https://github.com/adriengibrat), Frontend developer @ [Oodrive](https://www.oodrive.com/)</small>

<!-- Hello Dhaya, this talk use the Presentation API ;) -->

<script src="https://kit.fontawesome.com/8f4e5c18cf.js" crossorigin="anonymous"></script>
<script src="https://unpkg.com/mermaid@8.4.6/dist/mermaid.min.js" crossorigin="anonymous"></script>
<script>mermaid.initialize({ startOnLoad:true, securityLevel:'loose', });</script>

## 🌐 A Network-based architecture

[Representational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer) (REST)
is the core **[architecture principle of the Web](https://www.youtube.com/watch?v=w5j2KwzzB-0&feature=youtu.be&t=12) 🎞️**.

<small>Also watch [past, present, and future of the Web](https://www.youtube.com/watch?v=EHzrk1j7z7Q) 🍿 by [Steve Klabnik](https://www.steveklabnik.com) 🦄.</small>

<!-- First video is 10`, second is 50` -->

## 🔗 tied to HTTP protocol

Defined by [Roy Fielding](https://roy.gbiv.com/),
author of the [HTTP 1.1 protocol](https://tools.ietf.org/html/rfc2068),
also involved in [HTML](https://web.archive.org/web/20121117033741/http://ftp.ics.uci.edu/pub/ietf/html/) and [URI](https://tools.ietf.org/html/rfc3986) standards.

* [![HTTP timeline](https://kinsta.com/wp-content/uploads/2016/04/http-timeline.png)](https://hpbn.co/http2/)

* <small>⏩ 2020: [HTTP 3](https://translate.google.com/translate?sl=fr&tl=en&u=https://kinsta.com/fr/blog/http3) = [QUIC](https://en.wikipedia.org/wiki/QUIC) + [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol) as 🔒 security & 🚄 transport layer.</small>

<!--
HTTP 2 & HTTP 3 are low layers optimizations for huge performance boost,
without large impact or breaking changes to application layers.
-->

## ☑️ with some rules

**[REST constraints](https://restfulapi.net/rest-architectural-constraints/)**

* ↕️ *Client–server*
* ↔️ **Uniform interface**
* 🔀 **Stateless**
* 🔂 **Cacheable**
* ⬆️ **Layered**
* [🔣 *Code on demand* <small>(optional)</small>]

<!--
We'll see why those simple constraints matters.
I Won't talk about "Client–server", it's pretty obvious, nor about "Code on demand".
Let's keep "Uniform interface" for later.
-->

### 🔀 Stateless + 🔂 Cache = 🌱 Scalable

* MVP
    <div class="mermaid">
    graph LR
        Client[fa:fa-laptop ] --> Server[fa:fa-server ]
        Server --> Database(fa:fa-database )
    </div>

* Prod
    <div class="mermaid">
    graph LR
        Client[fa:fa-laptop ] --> Firewall{fa:fa-fire }
        Firewall --> LoadBalancer{fa:fa-network-wired }
        LoadBalancer --> |load| FrontalServer1[fa:fa-server ]
        LoadBalancer --> |balanced| FrontalServer2[fa:fa-server ]
        FrontalServer1 --> |cached| Cache{fa:fa-hdd }
        FrontalServer2 --> |responses| Cache
        FrontalServer1 --> Database(fa:fa-database )
        FrontalServer2 --> Database(fa:fa-database )
        Cache --> ExpensiveService(fa:fa-tachometer-alt )
    </div>

<!--
Enables intermediate processing by constraining messages to be self-descriptive: interaction is stateless between requests, standard methods and media types are used to indicate semantics and exchange information, and responses explicitly indicate cacheability.
-->

### 🔂 Cache + ⬆️ Layers = 🚀 Performance

<div class="mermaid">
    graph LR
        Client[fa:fa-laptop ] --> Firewall{fa:fa-fire }
        Firewall--> Gateway{fa:fa-network-wired }
        Gateway --> |rate| AnotherCache{fa:fa-hdd }
        Gateway --> |limited| LoadBalancer{fa:fa-network-wired }
        AnotherCache --> |debounce| AnotherFrontalServer[fa:fa-server ]
        AnotherFrontalServer --> AnotherDatabase(fa:fa-database )
        Gateway -.-> |monitor| ExpensiveService
        LoadBalancer --> FrontalServer1[fa:fa-server ]
        LoadBalancer --> FrontalServer2[fa:fa-server ]
        FrontalServer1 --> Cache{fa:fa-hdd }
        FrontalServer2 --> Cache
        FrontalServer1 --> Database(fa:fa-database )
        FrontalServer2 --> Database(fa:fa-database )
        Cache --> ExpensiveService(fa:fa-tachometer-alt )
</div>

<!--
Allow intermediaries—proxies, gateways, and firewalls—to be introduced at various points in the communication without changing the interfaces between components, thus allowing them to assist in communication translation or improve performance via large-scale, shared caching.
-->

### ↔️ Uniform interface

**🤘 [SOLID for API](https://stackoverflow.com/questions/25172600/rest-what-exactly-is-meant-by-uniform-interface)**

* Identify **resources** <small>JSON, CSV, XML or HTML != DB Schema</small>

* Manipulate **representations** <small>create, read, update, delete...</small>

* **Self-descriptive** messages <small>MIME type, cache, status...</small>

* Hypermedia **links** between **operations** <small>[OAI Links](https://swagger.io/docs/specification/links/), [json:api](https://jsonapi.org)</small>

<!--
- modifiability of components to meet changing needs (even while the application is running)
- visibility of communication between components by service agents
- portability of components by moving program code with the data
- reliability in the resistance to failure at the system level in the presence of failures within components, connectors, or data
-->

## 🎓 Take your time

> The effort required to design something is inversely proportional to the simplicity of the result.
[…] [REST is very simple](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven).

[![height:6em Leonard Richardson REST Maturity Model](https://reflectoring.io/assets/img/posts/rest-hypermedia/rest-maturity-model.jpg)](https://martinfowler.com/articles/richardsonMaturityModel.html)

<!--
A model, developed by Leonard Richardson, that breaks down the principal elements of a REST approach into three steps. These introduce resources, http verbs, and hypermedia controls.
-->

## 🍱 Takeway

[**5 simple rules**](https://translate.google.com/translate?sl=fr&tl=en&u=https://blog.nicolashachet.com/developpement-php/larchitecture-rest-expliquee-en-5-regles)

1) 🆔 URI = resources identifier
2) 🕹️ HTTP verbs = operation
3) 🧾 Responses = resources representation
4) 🔗 Links = relation between resources / operations
5) 🔑 Authentication tokens = parameter

<!-- Take a mental (or actual) picture -->

### 🙋 but you already know it!

### 🆔 URI

* 🅰️ /books/filter/polar/sort/asc
* 🅱️ /books?filter=polar&sort=asc

<!-- 🅱️ -->

### 🕹️ Verbs

* 🅰️ `PATCH` /books/42
* 🅱️ `POST` /books/edit/42

<!-- 🅰️ -->

### 🧾 Representations

    GET /books
    *request header*❓: application/json

* 🅰️ Content-Type
* 🅱️ Accept

<!-- 🅱️ -->

### 🔗 Links

* 🅰️ Link: </books?offset=2>; page="2"
* 🅱️ Link: </books?offset=2>; rel="next"

<!-- 🅱️ -->

### 🔑 Tokens

* 🅰️ Authorization: Bearer YWxh…
* 🅱️ ?token=zmlP…
* 🆎 *secure in URL, if short lived/single usage token*

<!-- 🆎 -->

### 🙂 Thank you

<!-- It's my last R&Day talk :/ See you @ Open R&Day -->
