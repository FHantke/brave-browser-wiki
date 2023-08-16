Giving sites access to specific hardware devices can create privacy risks, and potentially allow the site to re-identify you across visits (even if you clear storage or change browsers). This re-identification can happen in several ways, including:

- Some hardware will have semi-identifiers (e.g. product numbers, model numbers) that, when combined with other semi-identifiers (e.g. region, operating system) allow your browser to be fingerprinted and thus re-identified (even if you clear cookies)
- Some hardware will have unique identifiers (e.g. serial numbers, device keys) that can your specific instance of that hardware. Sites that can read unique identifiers will likely be able to re-identify you across sites, sessions, or even browsers

Brave protects against these privacy risks in a range of ways, including:

- Brave puts most information about your hardware behind [permission prompts](https://brave.com/privacy-updates/8-grab-bag-2/#improved-web-permission-lifetimes), so you can restrict which sites have access to it
- Where possible, Brave will [remove or randomize unique identifiers on hardware](https://github.com/brave/brave-browser/issues/28146), though note that this is not always possible: Hardware may present unique and semi-unique identifiers to websites in unpredictable ways that Brave cannot always obscure

Internet users should be careful and conservative when deciding which (if any) sites to give access to their hardware. While Brave includes best-effort privacy protections, Brave cannot entirely remove the privacy risk that comes from sharing such device-level information.