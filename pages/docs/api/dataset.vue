<template>
  <div>
    <Page :title="title" :side="side" no-head>
      <h1 id="dataset-api" class="mb-4">
        Dataset API
        <a href="#dataset-api"><v-icon color="accent">mdi-link</v-icon></a>
      </h1>

      <p>
        Purchasing a dataset requires two requests: one to obtain a quote and
        one to confirm the order. Both requests take a variable amount of time
        to complete and require a callback URL to handle the response.
      </p>

      <p>
        Transactional emails including order confirmations, receipts and
        invoices, will be send to the email address associated with the
        subscription in addition to the callbacks.
      </p>

      <h2 id="endpoint-request-quote" class="mt-8 mb-2">
        Endpoint: request quote
        <a href="#endpoint-request-quote"
          ><v-icon color="accent">mdi-link</v-icon></a
        >
      </h2>

      <p>
        <code>POST</code> <code>https://api.wappalyzer.com/dataset/v1/</code>
      </p>

      <h2 id="properties" class="mt-8 mb-2">
        Properties
        <a href="#properties"><v-icon color="accent">mdi-link</v-icon></a>
      </h2>

      <v-card class="my-4" flat outlined>
        <v-simple-table>
          <thead>
            <tr>
              <th>Property</th>
              <th>Description</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Execution</td>
              <td>Asynchronous</td>
            </tr>
            <tr>
              <td>Request timeout</td>
              <td>20s</td>
            </tr>
            <tr>
              <td>Rate limit</td>
              <td>1 request / second</td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-card>

      <h2 id="request-body-json" class="mt-8 mb-2">
        Request body (JSON)
        <a href="#request-body-json"
          ><v-icon color="accent">mdi-link</v-icon></a
        >
      </h2>

      <v-card class="my-4" flat outlined>
        <v-simple-table>
          <thead>
            <tr>
              <th>Name</th>
              <th>Required</th>
              <th>Description</th>
              <th>Example</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><code>technologies</code></td>
              <td>Yes</td>
              <td>
                Array of technology names (refer to the full list)
              </td>
              <td><code>["Craft CMS"]</code></td>
            </tr>
            <tr>
              <td><code>languages</code></td>
              <td>No</td>
              <td>
                Array of language tags to filter by
              </td>
              <td><code>["en-US"]</code></td>
            </tr>
            <tr>
              <td><code>tlds</code></td>
              <td>No</td>
              <td>
                Array of top-level domains to filter by
              </td>
              <td><code>[".com"]</code></td>
            </tr>
            <tr>
              <td><code>matchLanguageTld</code></td>
              <td>No</td>
              <td>
                Match language and top-level domain (yields fewer results)
              </td>
              <td><code>true</code></td>
            </tr>
            <tr>
              <td><code>subset</code></td>
              <td>No</td>
              <td>Limit results to <em>n</em> top traficked websites</td>
              <td><code>1000</code></td>
            </tr>
            <tr>
              <td><code>callback_url</code></td>
              <td>Yes</td>
              <td>
                A POST request will be made to the callback URL upon completion
                of the request
              </td>
              <td><code>https://example.com</code></td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-card>

      <h2 id="examples" class="mt-8 mb-2">
        Examples
        <a href="#examples"><v-icon color="accent">mdi-link</v-icon></a>
      </h2>

      <p><strong>Example request</strong></p>

      <pre
        class="mb-4"
      ><Prism language="bash" class="body-2">curl -H "x-api-key: your.api.key" \
-d '{ "technologies": ["Craft CMS"], "callbackUrl": "https://example.com" }' \
"https://api.wappalyzer.com/dataset/v1/"</Prism></pre>

      <p><strong>Example response (<code>202</code>)</strong></p>

      <pre class="mb-4"><Prism language="json" class="body-2">{
  "orderRef": "ABCDEFGHIJK"
}</Prism></pre>

      <p><strong>Example callback response</strong></p>

      <pre class="mb-4"><Prism language="json" class="body-2">{
  "status": "new"
  "description": "Dataset, 1,000 records",
  "amount": 70,
  "currency": "AUD",
  "records": {
    "counts": [
      "Craft CMS": 1000
    ],
    "subset": 1000
  },
  "query": {
    "technologies": [
      "Craft CMS"
    ]
    "languages": [
      "en-US"
    ],
    "tlds": [
      ".com"
    ],
    "matchLangTld": true
  },
  "dataset": {
    "url": null
  }
}</Prism></pre>

      <h2 id="endpoint-confirm-order" class="mt-8 mb-2">
        Endpoint: confirm order
        <a href="#endpoint-confirm-order"
          ><v-icon color="accent">mdi-link</v-icon></a
        >
      </h2>

      <p>
        <code>POST</code>
        <code>https://api.wappalyzer.com/dataset/v1/{orderRef}</code>
      </p>

      <p>
        To confirm a purchase, use the order reference
        <code>orderRef</code> returned by the previous API call to make a second
        request. Within minutes, the completed order details will be sent to the
        callback URL, including a link to download the dataset.
      </p>

      <v-alert icon="mdi-lightbulb-on-outline" outlined>
        The quoted amount will be charged to the credit card associated with
        your subscription.
      </v-alert>

      <h2 id="properties-2" class="mt-8 mb-2">
        Properties
        <a href="#properties-2"><v-icon color="accent">mdi-link</v-icon></a>
      </h2>

      <v-card class="my-4" flat outlined>
        <v-simple-table>
          <thead>
            <tr>
              <th>Property</th>
              <th>Description</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Execution</td>
              <td>Asynchronous</td>
            </tr>
            <tr>
              <td>Request timeout</td>
              <td>20s</td>
            </tr>
            <tr>
              <td>Rate limit</td>
              <td>
                1 request / 10 seconds (burstable to 1 request / second)
              </td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-card>

      <h2 id="examples-2" class="mt-8 mb-2">
        Examples
        <a href="#examples-2"><v-icon color="accent">mdi-link</v-icon></a>
      </h2>

      <p><strong>Example request</strong></p>

      <pre
        class="mb-4"
      ><Prism language="bash" class="body-2">curl -H "X-Api-Key: your.api.key" -X POST \
https://api.wappalyzer.com/dataset/v1/ABCDEFGHIJK</Prism></pre>

      <p><strong>Example response (<code>202</code>)</strong></p>

      <pre class="mb-4"><Prism language="json" class="body-2">{
{
  "orderStatus": "paid"
}</Prism></pre>

      <p><strong>Example callback response</strong></p>

      <pre class="mb-4"><Prism language="json" class="body-2">{
  "status": "complete"
  "description": "Dataset, 1,000 records",
  "amount": 70,
  "currency": "AUD",
  "records": {
    "counts": [
      "Craft CMS": 1000
    ],
    "subset": 1000
  },
  "query": {
    "technologies": [
      "Craft CMS"
    ]
    "languages": [
      "en-US"
    ],
    "tlds": [
      ".com"
    ],
    "matchLangTld": true
  },
  "dataset": {
    "url": "https://www.wappalyzer.com/files/wappalyzer-abcd-210101-ABCDEFGHIJK.zip"
  }
}</Prism></pre>
    </Page>
  </div>
</template>

<script>
import Page from '~/components/Page.vue'
import side from '~/assets/json/nav/docs.json'

export default {
  components: {
    Page
  },
  data() {
    return {
      title: 'Developer documentation',
      side
    }
  }
}
</script>
