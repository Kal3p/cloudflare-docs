---
pcx_content_type: how-to
title: Custom domains
---

# Custom domains

When deploying your Pages project, you may wish to point custom domains (or subdomains) to your site.

{{<Aside type="note" header="Purging the cache">}}

If Page Rules or other cache settings are used on your custom domain, that may lead to stale assets being served after a new build. Resolve this by selecting **Caching** > **Configuration** > [**Purge Everything**](/cache/how-to/purge-cache/purge-everything/) in the dashboard to ensure the latest build gets served.

{{</Aside>}}

## Add a custom domain

To add a custom domain:

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/login).
2. Select your account in **Account Home** > **Workers & Pages**.
3. Select your Pages project > **Custom domains**.
4. Select **Set up a domain**.
5. Provide the domain that you would like to serve your Cloudflare Pages site on and select **Continue**.

![Adding a custom domain for your Pages project through the Cloudflare dashboard](/images/pages/platform/domains.png)

### Add a custom apex domain

If you are deploying to an apex domain (for example, `example.com`), then you will need to add your site as a Cloudflare zone and [configure your nameservers](#configure-nameservers).

#### Configure nameservers

To use a custom apex domain (for example, `example.com`) with your Pages project, [configure your nameservers to point to Cloudflare's nameservers](/dns/zone-setups/full-setup/setup/). If your nameservers are successfully pointed to Cloudflare, Cloudflare will proceed by creating a CNAME record for you.

### Add a custom subdomain

If you are deploying to a subdomain, it is not necessary for your site to be a Cloudflare zone. You will need to [add a custom CNAME record](#add-a-custom-cname-record) to point the domain to your Cloudflare Pages site. To deploy your Pages project to a custom apex domain, that custom domain must be a zone on the Cloudflare account you have created your Pages project on.

{{<Aside type="note">}}

If the zone is on the Enterprise plan, make sure that you [release the zone hold](/fundamentals/setup/account/account-security/zone-holds/#release-zone-holds) before adding the custom domain. A zone hold would prevent the custom subdomain from activating.

{{</Aside>}}

#### Add a custom CNAME record

If you do not want to point your nameservers to Cloudflare, you must create a custom CNAME record to use a subdomain with Cloudflare Pages. After logging in to your DNS provider, add a CNAME record for your desired subdomain, for example, `shop.example.com`. This record should point to your custom Pages subdomain, for example, `<YOUR_SITE>.pages.dev`.

| Type    | Name               | Content                 |
| ------- | ------------------ | ----------------------- |
| `CNAME` | `shop.example.com` | `<YOUR_SITE>.pages.dev` |

If your site is already managed as a Cloudflare zone, the CNAME record will be added automatically after you confirm your DNS record.

{{<Aside type="note">}}

To ensure a custom domain is added successfully, you must go through the [Add a custom domain](#add-a-custom-domain) process described above. Manually adding a custom CNAME record pointing to your Cloudflare Pages site - without first associating the domain (or subdomains) in the Cloudflare Pages dashboard - will result in your domain failing to resolve at the CNAME record address, and display a [`522` error](/support/troubleshooting/cloudflare-errors/troubleshooting-cloudflare-5xx-errors/#error-522-connection-timed-out).

{{</Aside>}}

## Delete a custom domain

To detach a custom domain from your Pages project, you must modify your zone's DNS records.

First, log in to the Cloudflare dashboard > select your account in **Account Home** > select your website > **DNS**.

Then, in **DNS** > **Records**:

1.  Locate your Pages project's CNAME record.
2.  Select **Edit**.
3.  Select **Delete**.

Next, in Account Home, go to **Workers & Pages**:

1.  In **Overview**, select your Pages project.
2.  Go to **Custom domains**.
3.  Select the **three dot icon** next to your custom domain > **Remove domain**.

After completing these steps, your Pages project will only be accessible through the `*.pages.dev` subdomain you chose when creating your project.

## Disable access to `*.pages.dev` subdomain

To disable access to your project's provided `*.pages.dev` subdomain:

1. Use Cloudflare Access over your previews (`*.{project}.pages.dev`). Refer to [Customize preview deployments access](/pages/configuration/preview-deployments/#customize-preview-deployments-access).

2. Redirect the `*.pages.dev` URL associated with your production Pages project to a custom domain. You can use the account-level [Bulk Redirect](/rules/url-forwarding/bulk-redirects/) feature to redirect your `*.pages.dev` URL to a custom domain.

## Known issues

### CAA records

Certification Authority Authorization (CAA) records allow you to restrict certificate issuance to specific Certificate Authorities (CAs).

This can cause issues when adding a [custom domain](/pages/configuration/custom-domains/) to your Pages project if you have CAA records that do not allow Cloudflare to issue a certificate for your custom domain.

To resolve this, add the necessary CAA records to allow Cloudflare to issue a certificate for your custom domain.

```
example.com.            300     IN      CAA     0 issue "comodoca.com"
example.com.            300     IN      CAA     0 issue "digicert.com; cansignhttpexchanges=yes"
example.com.            300     IN      CAA     0 issue "letsencrypt.org"
example.com.            300     IN      CAA     0 issue "pki.goog; cansignhttpexchanges=yes"
example.com.            300     IN      CAA     0 issuewild "comodoca.com"
example.com.            300     IN      CAA     0 issuewild "digicert.com; cansignhttpexchanges=yes"
example.com.            300     IN      CAA     0 issuewild "letsencrypt.org"
example.com.            300     IN      CAA     0 issuewild "pki.goog; cansignhttpexchanges=yes"
```

Refer to the [Certification Authority Authorization (CAA) FAQ](/ssl/edge-certificates/troubleshooting/caa-records/) for more information.

### Change DNS entry away from Pages and then back again

Once a custom domain is set up, if you change the DNS entry to point to something else (for example, your origin), the custom domain will become inactive. If you then change that DNS entry to point back at your custom domain, anybody using that DNS entry to visit your website will get errors until it becomes active again. If you want to redirect traffic away from your Pages project temporarily instead of changing the DNS entry, it would be better to use an [Origin rule](/rules/origin-rules/) or a [redirect rule](/rules/url-forwarding/single-redirects/create-dashboard/) instead.

## Relevant resources

- [Debugging Pages](/pages/configuration/debugging-pages/) - Review common errors when deploying your Pages project.