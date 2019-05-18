<img src=image.png>

# Suggestions to Improve TaxJar Assets

Hello to anyone from TaxJar who has made their way to viewing this! 👋

The purpose of this document is to list several improvements that came to mind while famiiliarizing myself with TaxJar's business (centered mostly around [developers.taxjar.com](https://developers.taxjar.com)).

Feel free to send a pull request to check off any items as they're completed.

### Legend 🗺️

|   |  |
| ------------- | ------------- |
| `[characters or words]`  | add or replace with `characters or words`  |
| `[-characters or words]`  | delete `characters or words`  |
| `[-]` | add a dash `-` |
| `[...]` | just an [ellipsis to indicate omission of part of the quote](https://english.stackexchange.com/questions/55820/how-would-you-properly-show-deletion-of-unnecessary-text-in-a-quot) - no change necessary


### Website 🌐


- https://app.taxjar.com/dashboard

  - [ ] No way to close self-guided tour `appcues` popup in Chrome 74.0.3729.131

  - [ ] Related typo:

    - "Guess what? We'll even give you a FREE AutoFile Credit[space]for completing our demo!"

- https://www.taxjar.com/states/

  - [ ] **Update React version** (here and anywhere else React is used) in case any bugfixes or performance improvements apply (perhaps use a tool to automate this; e.g., Greenkeeper or Renovate).
 
- http://www.fbasalestax.com/

  - [ ] **Implement HTTPS** ("Not Secure" in Chrome)

- https://developers.taxjar.com/api/guides/#cart-integrations

  - [ ] This section currently only lists the following integrations:
    - Magento
    - Magento 2
    - Stripe
    - WooCommerce
  - Should the other integrations be listed here as well? E.g.:
    - Amazon
    - Paypal
    - etc.
  - Or, is this section strictly for “cart integrations” and not marketplace integrations? In that case, would at least the following apply?
    - Square
    - Squarespace
    - Shopify
    - BigCommerce
    - Ecwid

- https://www.taxjar.com/states/california-sales-tax-online/#how-to-collect-sales-tax-in-california

  - [ ] "Here’s the [state’s official Sales and Use Tax rates PDF](http://www.boe.ca.gov/pdf/pub71.pdf), also known as Publication 71."
  
    - It seems the BOE no longer administers the sales/use tax in CA and this link is no longer valid.
    
    - Perhaps link to http://www.cdtfa.ca.gov/taxes-and-fees/rates.aspx from the CDTFA (not Publication 71).

  - [ ] "Once you’ve figured out how much [-to] sales tax to charge to your customers, it’s time to report and file.”

- https://www.taxjar.com/states/california-sales-tax-online/#when-are-california-sales-tax-returns-due

  - [ ] see the [California monthly prepayment due dates here](http://www.boe.ca.gov/sutax/fill_dates.htm).

    - BOE link is no longer valid

    - consider linking to https://www.cdtfa.ca.gov/taxes-and-fees/sales-use-tax-returns-filing-dates.htm

  - [ ] The 2019 California Monthly Sales Tax Filing Due Dates table seems to be calculating incorrectly.

    - According to the above link “Returns are due at the end of the month for the previous month.”

    - E.g., for June the due date is July 31.

    - The following months appear to be incorrect in the table:

      - Feb
      - May
      - Jul
      - Oct

- https://www.taxjar.com/states/california-sales-tax-online/#how-to-file-sales-tax-in-california

  - [ ] "1. File online – [File online at the California Board of Equalization](https://efile.boe.ca.gov/boe/boe_login.jsp)."

    - Will want to update this to a CDTFA link when appropriate.

  - [ ] “2. File by mail – You can use [California’s short form sales and use tax return](http://www.boe.ca.gov/pdf/boe401ez.pdf) and file through the mail, […].”

    - Update BOE link to https://www.cdtfa.ca.gov/DownloadFile.ashx?path=/formspubs/cdtfa401ez.pdf

  - [ ] “[Late] Payment Penalty – 10% of sales tax owed (Source)”

- https://www.taxjar.com/states/california-sales-tax-online/

  - [ ] "State Resources” in sidebar

    - Will want to update BOE links here as well.
 
### SmartCalcs API (Perhaps update with next version)

- [ ] GET https://api.taxjar.com/v2/categories

```
{
  "product_tax_code": "40030",
  "name": "Food & Groceries",
  "description": "Food for human[-s] consumption, unprepared”
},
{
  "product_tax_code": "30070",
  "name": "Software as a Service",
  "description": "Pre-written software, delivered electronically, but access[ed] remotely.”
}
```

- [ ] GET https://api.taxjar.com/v2/summary_rates

  - Consider updating `summary_rates` in response to a JSON object rather than a JSON array.

  - That way, integrations/users can look up a rate in constant rather than linear time after parsing the response.

  - One possibility is to concatenate `country_code` and `region_code` for each key; e.g.:

```
{
  “summary_rates”: {
    “US_CA”: {
      “country_code”: “US”,
      “country”: “United States”,
      “region_code”: “CA”,
      “region”: “California”,
      “minimum_rate”: {
        “label”: “State Tax”,
        “rate”: 0.065
      },
      “average_rate”: {
        “label”: “Tax”,
        “rate”: 0.0827
      }
    }
  }
}
```

  - (Note: this would result in a key of `UK_null` for UK VAT tax and other, similar entries.)

  - This way you could look up the rate in Node.js, for example, with:

```js
JSON.parse(res).summary_rates.US_CA.average_rate.rate; // O(1) constant time
```

  - Rather than something like:

```js
JSON.parse(res).summary_rates.find(({country_code, region_code}) => country_code === ‘US' && region_code === ‘CA’).average_rate.rate; // O(n) linear time
```

- [ ] POST https://api.taxjar.com/v2/addresses/validate

  - When sending a request from a basic plan (and not TaxJar Plus),

    - ~~send a `403` - Forbidden or `401` - Not Authorized error code~~

    - ~~rather than a `500` - Internal Server Error error code.~~

    - See issue [#1](https://github.com/ScottRudiger/TaxJar-Suggestions/issues/1); this is related to Postman usage and not the API itself.

    - Respond with the following to be consistent with the attributes provided by other errors sent by the API:
    ```json
    {
      "error": "Not Authorized",
      "detail": "Upgrade to Plus to use this feature",
      "status": "401"
    }
    ```
    - rather than:
    ```json
    {
      "errors": "Not Authorized",
      "body": "Upgrade to Plus to use this feature"
    }
    ```

### Typos 😱

- https://app.taxjar.com/smartcalcs

  - [ ] typo in TaxJar Plus pop-up (Sorry, I can't remember what the exact typo is as the pop-up no longer shows up for me.)

- https://www.taxjar.com/amazon-fba-sales-tax/

  - [ ] "We give the [-the] exact data you need to be able to file your tax returns much faster than using Excel spreadsheets."

- https://support.taxjar.com/article/30-expected-sales-tax-due-faq

  - "I make tax exempt sales. Why does my Dashboard […]?”

    - [ ] 2. has duplicate text in common with 2a.; delete the duplicate text in 2. and add the dash in "You sell non-taxable (exempt) or reduced[-]tax items."

    - [ ] 2a.: "If your products fall into tax[-]exempt or reduced[-]tax categories,* you can tell TaxJar more about which items are exempt so [-both] the Expected Report (and AutoFile) will recognize which sales should be exempt."

    - [ ] 2c.: "reduced[-]tax"

  - [ ] "If you make sales on platforms outside of Amazon, Shopify and SmartCalcs and you sell exempt or reduced[-]tax items [...]."
  
  - "Q: Are refunds included in the Adjustments/Presumed exempt totals?”

    - “In other state Reports, if a return has already been filed, and a refund takes place […].”
    
      - [ ] Clarify between refunds to customers and refunds from the taxing entity; e.g., by updating to something like:
      
        - "In other state Reports, if a return has already been filed, and a refund takes place after the return is filed, the state may require you to file an amended return [to claim a refund for sales tax remitted], rather than [deducting that amount] from the next filing period’s return."

- https://blog.taxjar.com/how-amazon-sales-tax-collection/

  - [ ] "We[-’] receive this question a lot, so we thought we would take a second to clear things up."

- https://blog.taxjar.com/cancel-sales-tax-registration/

  - [ ] "While [that] may be appropriate from your perspective, the state where you are registered […]."

  - [ ] "[…] which may require that tax be collected and returns [be] filed for 6 months or longer […]."

  - [ ] "The states [-will to] assert this position by arguing that the sales activity or the other nexus[-]creating activities your business had in the states has established a market[delete space]place for your product in their state [and] will continue to generate sales even after these nexus[-]creating activities have stopped."

  - [ ] "[…] historic nexus[-]creating actions."

  - [ ] "[…] and assert [that] the historical sales activities conducted by your company […]."

  - [ ] "If your company’s nexus[-]creating activity in a state was the presence of an employee who had [-no] nothing to do with soliciting sales[,] then the concept of residual or trailing nexus may not apply."

- https://blog.taxjar.com/sales-tax-digital-products/

  - [ ] "For the purposes of this blog post, we are going to focus on [-focus on] digital products […]."

- https://blog.taxjar.com/streamlined-sales-tax-project-what-is-it-and-how-does-it-affect-your-business/

  - [ ] "The efforts of s[t]ates to collect tax on these out-of-state sellers [...]."

  - [ ] "[...] and a uniform tax registration number that is [us]ed to file returns in the member states."

  - [ ] "[...] if they agreed to register in [-register in] all of the member states and remain registered for a period of 36 months."

  - [ ] "[...] uniformity with [-a] the model code of the SSTP."

  - [ ] "However, the SSTP requires that certain administrative provisions be adopted [-will] that impact all businesses."

  - [ ] "In addition, there are special provisions about sales tax holiday[-’]s [...]."

  - [ ] "[...] that the SSTP rules and new statu[t]es are having on tax administrators."

- https://life.taxjar.com/best-deal-never-made/

  - [ ] "[...] put ou[r] customers above everything else [...]."

  - [ ] "[...] employ[-ee] dozens of people on a distributed team [...]."

- https://life.taxjar.com/moment-changed-everything-taxjar/

  - [ ] "We we[re] starving and had time to kill [...]."

- https://www.taxjar.com/autofile/

  - "What do I need to enroll in AutoFile?"
  
    - [ ] […] a US[-]based bank account.

  - "Where can I read more about AutoFile?"

    - [ ] "Here are even more common questions and answers about AutoFile from other TaxJar customers[.]"

- https://developers.taxjar.com/integrations/onboarding/

  - [ ] "It shouldn’t have to compete for attention with other call[s] to action[-s]."

- https://developers.taxjar.com/integrations/sales-tax-calculations/

  - Store Location

    - [ ] "Here’s what Graham[,] our sales tax analyst[,] has to say:"

- https://developers.taxjar.com/integrations/sales-tax-reporting/

  - [ ] "Alternatively, you can allow them [to] export their old transactions to a CSV file […].”

  - [ ] "To backfill transactions through SmartCalcs, you should filter a collection of order transactions by an `updated_at` or `created_at` date range [-and] based on the API guidelines mentioned below.”

- https://developers.taxjar.com/integrations/testing/

  - [ ] "Order transaction[-s] [that] include[s] line item discounts"
  
  - [ ] "Order transaction[-s] [that] include[s] shipping discounts"

- https://developers.taxjar.com/integrations/faq/

  - [ ] “What [are] your API uptime and response times?”

- https://www.taxjar.com/states/california-sales-tax-online/#do-you-have-sales-tax-nexus-in-california

  - [ ] 4. "Presence at a tradeshow – Making sales at a tradeshow may [constitute] nexus"

- https://www.taxjar.com/states/california-sales-tax-online/#do-you-have-economic-nexus-in-california

  - [ ] Perhaps specify that this applies to sales into California, e.g.:

    - "Effective April 1, 2019, California considers retailers who make more than $100,000 in taxable annual sales [in the state] or conduct more than 200 transactions annually [in the state] to have economic nexus."
