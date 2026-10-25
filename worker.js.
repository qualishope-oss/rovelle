
const BASE_URL = "https://rovelle.qualishope.workers.dev";
const TOTAL_PRODUCTS = 50000;

const titles = [
  "Audifort Official Product Information",
  "Audifort Purchase Options and Details",
  "Audifort Product Guide",
  "Audifort Available Packages",
  "Audifort Online Shopping Information",
  "Audifort Product Overview",
  "Audifort Package Selection Guide",
  "Audifort Buying Information",
  "Audifort Product Details and Options",
  "Audifort Order Information",
  "Audifort Shopping Guide",
  "Audifort Package Comparison",
  "Audifort Product Features",
  "Audifort Purchase Guide",
  "Audifort Available Bundle Information",
  "Audifort Online Product Details",
  "Audifort Customer Buying Guide",
  "Audifort Product Selection",
  "Audifort Bundle Options",
  "Audifort Product and Ordering Guide"
];

const descriptions = [
  "Explore Audifort product information and available purchasing options.",
  "Learn more about Audifort packages before making your purchase.",
  "Review Audifort product details and visit the main shopping page.",
  "Discover Audifort purchasing information and package options.",
  "Find helpful information about Audifort and its available bundles.",
  "Explore the Audifort product overview and buying guide.",
  "Compare the available Audifort package options.",
  "Review Audifort ordering information before choosing a package.",
  "Visit the Audifort product page for current purchase details.",
  "Learn about Audifort and review the available buying options.",
  "Explore Audifort product information and shopping details.",
  "Find Audifort package information in one convenient location.",
  "Read about Audifort before visiting the main purchase page.",
  "Discover available Audifort bundles and product information.",
  "Review the Audifort buying guide and product details.",
  "Explore Audifort shopping information and available options.",
  "Find Audifort product details and purchase information.",
  "Learn about Audifort package choices and ordering details.",
  "Review Audifort product information and available packages.",
  "Visit the Audifort main page to explore purchasing options."
];

function generateProduct(id) {
  const titleIndex = (id - 1) % titles.length;

  const descriptionIndex =
    Math.floor((id - 1) / titles.length) % descriptions.length;

  const productId = String(id).padStart(5, "0");

  return {
    id: productId,
    title: `${titles[titleIndex]} | Reference ${productId}`,
    description: descriptions[descriptionIndex],
    url: `${BASE_URL}/?id=audifort-${productId}`
  };
}

function escapeHTML(value) {
  return String(value)
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;")
    .replaceAll("'", "&#39;");
}

function productPage(product) {
  return `<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport"
content="width=device-width, initial-scale=1.0">

<title>${escapeHTML(product.title)}</title>

<meta name="description"
content="${escapeHTML(product.description)}">

<meta name="robots" content="noindex,follow">

<link rel="canonical" href="${BASE_URL}/">

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 20px;
  background: #f4f7fb;
  font-family: Arial, sans-serif;
  color: #18243b;
}

.container {
  max-width: 850px;
  margin: 50px auto;
  background: #ffffff;
  padding: 40px;
  border-radius: 16px;
  box-shadow: 0 5px 25px rgba(0,0,0,.08);
}

h1 {
  font-size: 32px;
  line-height: 1.3;
}

p {
  font-size: 17px;
  line-height: 1.7;
}

.button {
  display: inline-block;
  padding: 15px 28px;
  background: #1769e0;
  color: #ffffff;
  border-radius: 8px;
  text-decoration: none;
  font-weight: bold;
}

.reference {
  font-size: 13px;
  color: #777;
}

.notice {
  font-size: 14px;
  color: #666;
  margin-top: 24px;
}
</style>
</head>

<body>
<main class="container">

<div class="reference">
Demo Reference: ${product.id}
</div>

<h1>${escapeHTML(product.title)}</h1>

<p>${escapeHTML(product.description)}</p>

<p>
Explore Audifort product information,
available packages, and purchase details.
</p>

<a class="button" href="${BASE_URL}/">
View Audifort Product
</a>

<p class="notice">
This is a demonstration catalog entry,
not a separate product listing.
</p>

</main>
</body>
</html>`;
}

function createDemoSitemap() {
  const encoder = new TextEncoder();

  let current = 0;

  const stream = new ReadableStream({
    pull(controller) {
      if (current === 0) {
        controller.enqueue(encoder.encode(
          '<?xml version="1.0" encoding="UTF-8"?>\n' +
          '<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">\n'
        ));
      }

      const batchSize = 250;
      const end = Math.min(current + batchSize, TOTAL_PRODUCTS);

      let chunk = "";

      for (let i = current + 1; i <= end; i++) {
        const product = generateProduct(i);

        chunk +=
          "<url><loc>" +
          escapeHTML(product.url) +
          "</loc></url>\n";
      }

      if (chunk) {
        controller.enqueue(encoder.encode(chunk));
      }

      current = end;

      if (current >= TOTAL_PRODUCTS) {
        controller.enqueue(encoder.encode("</urlset>"));
        controller.close();
      }
    }
  });

  return new Response(stream, {
    headers: {
      "Content-Type": "application/xml; charset=UTF-8",
      "Cache-Control": "public, max-age=3600",
      "X-Robots-Tag": "noindex"
    }
  });
}

function createPublicSitemap() {
  const xml = `<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<url>
<loc>${BASE_URL}/</loc>
</url>
</urlset>`;

  return new Response(xml, {
    headers: {
      "Content-Type": "application/xml; charset=UTF-8"
    }
  });
}

function createCatalogJSON() {
  const encoder = new TextEncoder();
  let current = 0;

  const stream = new ReadableStream({
    pull(controller) {
      if (current === 0) {
        controller.enqueue(encoder.encode("["));
      }

      const end = Math.min(current + 100, TOTAL_PRODUCTS);
      let chunk = "";

      for (let i = current + 1; i <= end; i++) {
        if (i > 1) chunk += ",";
        chunk += JSON.stringify(generateProduct(i));
      }

      controller.enqueue(encoder.encode(chunk));
      current = end;

      if (current >= TOTAL_PRODUCTS) {
        controller.enqueue(encoder.encode("]"));
        controller.close();
      }
    }
  });

  return new Response(stream, {
    headers: {
      "Content-Type": "application/json; charset=UTF-8",
      "X-Robots-Tag": "noindex"
    }
  });
}

export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    const path = url.pathname;

    if (path === "/sitemap.xml") {
      return createPublicSitemap();
    }

    if (path === "/demo-sitemap.xml") {
      return createDemoSitemap();
    }

    if (path === "/catalog.json") {
      return createCatalogJSON();
    }

    const id = url.searchParams.get("id");

    if (path === "/" && id !== null) {
      const match = /^audifort-(\d{1,5})$/.exec(id);

      if (!match) {
        return new Response("Product not found", {
          status: 404
        });
      }

      const number = Number(match[1]);

      if (number < 1 || number > TOTAL_PRODUCTS) {
        return new Response("Product not found", {
          status: 404
        });
      }

      const product = generateProduct(number);

      return new Response(productPage(product), {
        headers: {
          "Content-Type": "text/html; charset=UTF-8",
          "X-Robots-Tag": "noindex",
          "Cache-Control": "public, max-age=3600"
        }
      });
    }

    // Preserve the original website when an upstream
    // origin is configured.
    if (env.ORIGIN_URL) {
      const origin = new URL(env.ORIGIN_URL);
      origin.pathname = url.pathname;
      origin.search = url.search;

      if (origin.hostname === url.hostname) {
        return new Response(
          "ORIGIN_URL must point to a different origin.",
          { status: 500 }
        );
      }

      return fetch(new Request(origin.toString(), request));
    }

    return new Response(
      "Original website origin is not configured.",
      { status: 503 }
    );
  }
};
