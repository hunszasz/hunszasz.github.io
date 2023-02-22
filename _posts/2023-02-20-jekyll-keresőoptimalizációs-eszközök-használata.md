---
layout: post
title: Jekyll keresőoptimalizációs eszközök használata
date: 2023-02-20 08:40 +0100
categories: [Működés, Hasznos]
---
# Cél

Korábbi ["GitHub oldal jekyll alapon" írás](/működés/hasznos/2023/01/19/jekyll-github-oldal-létrehozása.html) kiegészítése Google SEO eszközök használatával.
Statikus oldal használata mellett szerettem volna látni, hogy a blogom milyen használati statisztikát mutat. Erre az alábbiakat találtam.

# Lépések

1. lépésben a ```Google Tag``` azonosítására lesz szükség a [Google analytics kezdeti lépések](/működés/hasznos/2023/02/20/google-analytics-kezdeti-lépések.html) posztban leírtak szerint.
2. jekyll ```_config.yml``` beállítófájlban szükséges felvenni a ```jekyll-seo-tag``` plugin-t, valamint meg kell adni a tracking id-t az alábbi formátumban
```yaml
plugins:
  - jekyll-seo-tag
google_analytics: "G-#########"
```
3. sitemap generálása többféle módon működhet, plugin és template alapon.
   1. pluginként a ```jekyll-sitemap``` kell a ```_config.yml``` állomány plugins szekciójában felvenni.
   2. templatként pedig a gyökérkönyvtárba kell a sitemap.xml-t az alábbi tartalommal felveni
   {% raw %}
   ```xml
   ---
           layout: null
           sitemap:
           exclude: 'yes'
           ---
           <?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
      {% for post in site.posts %}
      {% unless post.published == false %}
      <url>
         <loc>{{ site.url }}{{ post.url }}</loc>
         {% if post.sitemap.lastmod %}
         <lastmod>{{ post.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
         {% elsif post.date %}
         <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
         {% else %}
         <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
         {% endif %}
         {% if post.sitemap.changefreq %}
         <changefreq>{{ post.sitemap.changefreq }}</changefreq>
         {% else %}
         <changefreq>daily</changefreq>
         {% endif %}
         {% if post.sitemap.priority %}
         <priority>{{ post.sitemap.priority }}</priority>
         {% else %}
         <priority>0.5</priority>
         {%- endif -%}
      </url>
      {% endunless %}
      {% endfor %}
      {% for page in site.pages %}
      {% unless page.sitemap.exclude == "yes" or page.url contains "assets" %}
      <url>
         <loc>{{ site.url }}{{ page.url | remove: "index.html" }}</loc>
         {% if page.sitemap.lastmod %}
         <lastmod>{{ page.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
         {% elsif page.date %}
         <lastmod>{{ page.date | date_to_xmlschema }}</lastmod>
         {% else %}
         <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
         {% endif %}
         {% if page.sitemap.changefreq %}
         <changefreq>{{ page.sitemap.changefreq }}</changefreq>
         {% else %}
         <changefreq>daily</changefreq>
         {% endif %}
         {% if page.sitemap.priority %}
         <priority>{{ page.sitemap.priority }}</priority>
         {% else %}
         <priority>0.5</priority>
         {% endif %}
      </url>
      {% endunless %}
      {% endfor %}
   </urlset>
   ```
   {% endraw %}
4. Google Search Console sitemap hozzáadása. Az első postom január 18-án keletkezett, az analitikai inicializációt tartalmazó commit-om január 30-án történt. 
Megoldást szerettem volna találni, hogy a Google mihamarabb indexelje le a blogom. Ennek a folyamatnak az egyik lehetséges megoldása, 
ha Search Console-ban kézzel megadjuk a sitemap-ünket.
   1. Webhelytérképek menüpont alól adjuk meg az elérést ```sitemap.xml```
   2. ellenőrizzük az állapotát
5. A Search Console felületen lehetőségünk van ellenőrizni az eredményt.
  ![sitemap](/assets/sitemap.webp)

Ezt követően már lehet sokmindent nézni a Google eszközökön. Ami nagyon meglepett, hogy a forgalomszerzés alatt nem csak ```direct``` csatornáról - beírták a blogom url-jét a böngészőbe - érkeztek látogatók.
A ```referral``` részről volt egy nagyon meglepő is, ```mail.google.com```. Az internet népe azt írja, hogy valaki elküldte a blog linkjét egy gmail-es címre, amit az olvasója meg is nyitott.
![forgalomszerzés](/assets/forgalomszerzés.webp)

A sitemap kézi beállítása további jótékony eredményeket is adott, megjelent a Google indexfájában a blog.
![google](/assets/google.webp)

# Felmerült problémák
A sitemap kézi felvitele közben az állapotnál folyamatosan érvénytelen url formátumra jelzett a rendszer. Ezt kicsit közelebről megvizsgálva, arra jutottam, hogy a protokol és host hiányzott az url-ekből.
A ```_config.yml```-be init során kimaradt lépést pótolva megoldódott a probléma.
```yaml
url: "https://hunszasz.github.io"
```

# Hasznos linkek
* [Jekyll sitemap](https://peteranglea.github.io/2018/08/31/jekyll-sitemap/)

